<script>
  import Rate from "svelte-rate-it/Rate.svelte";
  import firebase from "@firebase/app";
  import {
    Row,
    Col,
    Select,
    ExpansionPanel,
    ExpansionPanels,
    ProgressCircular,
    TextField,
    Button
  } from "svelte-materialify";
  export let courses;
  export let userDocRef;
  export let ratingsColl;
  let creditsFilter;
  let creditsFilterOptions;
  let GERFilter;
  let subjectFilter;
  let subjectFilterOptions;
  let GERFilterOptions;
  let filteredCourses;
  let searchFilter;

  //only show options that are valid
  function returnOptions(param) {
    return [
      ...new Set(
        courses.filter(course => course[param]).map(course => course[param])
      )
    ];
  }
  //$: are reactive variables (variables change dynamically)
  $: creditsFilterOptions = returnOptions("credits");
  $: GERFilterOptions = returnOptions("GERCategory");
  $: subjectFilterOptions = returnOptions("subject");

  //filter courses based on user's filters
  function isEmpty(array) {
    return !(Array.isArray(array) && array.length);
  }
  $: filteredCourses = courses.filter(
    course =>
      (isEmpty(creditsFilter) || creditsFilter.includes(course.credits)) &&
      (isEmpty(GERFilter) || GERFilter.includes(course.GERCategory)) &&
      (isEmpty(subjectFilter) ||
        subjectFilter.includes(course.subjectFilter)) &&
      (searchFilter == undefined ||
        course.name
          .toLowerCase()
          .trim()
          .includes(searchFilter.toLowerCase().trim()))
  );

  //processing ratingsColl to get average ratings
  function averageArray(array) {
    let sum = 0;
    let i = 0;
    for (; array[i]; i++) {
      sum += array[i];
    }
    return sum / i;
  }
  /*function to find average rating.
  While the code may be complicated, 
  it is the only way to get average user ratings across multiple users
  with the fastest possible efficiency of O(n) as it uses hashtables*/
  /* takes collection of ratings (ratingsColl),
  and returns data in the following structure:
  [
    {"courseCode": "SYD393", "avgRating": 2},
    {"courseCode": "STA244", "avgRating": 4.25},
  ]
  */
  function getAverageRatings(ratingsColl) {
    return Object.entries(
      ratingsColl
        .map(userRatings => userRatings.ratings)
        .flat()
        .reduce((c, i) => {
          if (!c.hasOwnProperty(i.courseCode)) {
            c[i.courseCode] = [];
          }
          c[i.courseCode].push(i.rating);
          return c;
        }, {})
    ).map(([k, i]) => {
      return { code: k, rating: averageArray(i) };
    });
  }
  // set average ratings to update dynamically using $: operator
  $: averageRatings = getAverageRatings(ratingsColl);
  //merging courses and ratings so that each course has a rating
  $: courses = courses.map((item, i) => {
    if (averageRatings[i] && item.code === averageRatings[i].code) {
      //merging two objects
      return Object.assign({}, item, averageRatings[i]);
    } else return item;
  });
  $: console.log(courses);

  //function to add a course to the database when user clicks "ADD" button
  function addCourse(courseCode) {
    console.log(courseCode);
    userDocRef.update(
      {
        courses: firebase.firestore.FieldValue.arrayUnion(courseCode)
      },
      { merge: true }
    );
  }
</script>

<!--Filters for courses-->
<Row>
  <Col>
    <TextField clearable bind:value={searchFilter}>Search</TextField>
  </Col>
  <Col>
    <Select
      chips
      multiple
      items={creditsFilterOptions}
      bind:value={creditsFilter}>
      Credits
    </Select>
  </Col>
  <Col>
    <Select chips multiple items={GERFilterOptions} bind:value={GERFilter}>
      GER Category
    </Select>
  </Col>
  <Col>
    <Select
      chips
      multiple
      items={subjectFilterOptions}
      bind:value={subjectFilter}>
      Subject
    </Select>
  </Col>
</Row>

<ExpansionPanels accordion>
  {#each filteredCourses as course, i}
    <ExpansionPanel>
      <span slot="header">{course.code}: {course.name}</span>
      <div style="display:flex; flex-direction:column">
        <div>{course.description}</div>

        <!--additional information-->
        <ExpansionPanels accordion>
          <ExpansionPanel>
            <span slot="header">Professors</span>
            <div style="display:flex; flex-direction:column">
              {#each course.professors as professor}
                <li>{professor}</li>
              {:else}
                <ProgressCircular indeterminate />
              {/each}
            </div>
          </ExpansionPanel>
        </ExpansionPanels>

        <div>
          <Button on:click={addCourse(course.code)}>Add</Button>
          {#if averageRatings[i]}
            <Rate value={averageRatings[i].rating} readonly={true} />
          {/if}
        </div>
      </div>
    </ExpansionPanel>
  {:else}No courses found...{/each}
</ExpansionPanels>
