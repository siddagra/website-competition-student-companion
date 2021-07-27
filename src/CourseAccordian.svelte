<script>
  import { ExpansionPanel, ExpansionPanels, Button } from "svelte-materialify";
  import Rate from "svelte-rate-it/Rate.svelte";
  import firebase from "@firebase/app";

  export let userCourse;
  export let user;
  export let userDocRef;

  //function to add a course to
  function removeCourse(courseCode) {
    console.log(courseCode);
    userDocRef.update(
      {
        courses: firebase.firestore.FieldValue.arrayRemove(courseCode)
      },
      { merge: true }
    );
  }

  //rating system logic
  function afterRate(rating) {
    var userRatingDocRef = firebase
      .firestore()
      .collection("ratings")
      .doc(user.uid);

    userRatingDocRef.get().then(doc => {
      //if document exists, add rating to the document
      if (doc.exists) {
        userRatingDocRef.update(
          {
            ratings: firebase.firestore.FieldValue.arrayUnion({
              courseCode: userCourse.code,
              rating: rating
            })
          },
          { merge: true }
        );
      }
      //if document does not exist, create document
      else {
        firebase
          .firestore()
          .collection("ratings")
          .doc(user.uid)
          .set({
            ratings: [
              {
                courseCode: userCourse.code,
                rating: rating
              }
            ]
          });
      }
    });

    console.log(rating);
    console.log(user.uid);
  }
</script>

<ExpansionPanel>
  <span slot="header">{userCourse.code}: {userCourse.name}</span>
  <div style="display:flex; flex-direction:column">
    <div>{userCourse.description}</div>

    <!--additional information-->
    <ExpansionPanels accordion>
      <ExpansionPanel>
        <span slot="header">Professors</span>
        <div style="display:flex; flex-direction:column">
          {#each userCourse.professors as professor}
            <li>{professor}</li>
          {:else}No professors assigned...{/each}
        </div>
      </ExpansionPanel>
    </ExpansionPanels>
    <div>
      <Button on:click={removeCourse(userCourse.code)}>Remove</Button>
      <Rate {afterRate} />
    </div>
  </div>
</ExpansionPanel>
