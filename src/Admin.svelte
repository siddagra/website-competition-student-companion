<script>
  import { subjects, GERCategories } from "./stores.js";
  import { firebase } from "@firebase/app";
  import {
    Row,
    Col,
    Select,
    TextField,
    Textarea,
    Button
  } from "svelte-materialify";
  export let coursesRef;
  let courseCode, courseName, courseDesc, credits, subject, GERCategory;

  //validation of all fields
  let validationArray = new Array(10);
  //declare rules for required fields
  const rules = [v => !!v || "Required"];
  //variable to handle button UI
  let validationError = false;

  function addCourse() {
    if (!validationArray.includes(false)) {
      let newCourseRef = coursesRef.doc();
      newCourseRef.set({
        code: courseCode,
        name: courseName,
        credits: parseFloat(credits),
        subject: subject,
        description: courseDesc,
        GERCategory: GERCategory,
        courseId: newCourseRef.id,
        createdTime: firebase.firestore.Timestamp.fromDate(new Date())
      });
    } else {
      validationError = true;
      setTimeout(function() {
        validationError = false;
      }, 1000);
    }
  }
</script>

<style>
  .container {
    min-height: 1000px;
  }
</style>

<div class="container">
  <Row>
    <Col>
      <TextField
        clearable
        {rules}
        bind:validation={validationArray[0]}
        bind:value={courseCode}>
        Course Code
      </TextField>
    </Col>
    <TextField
      clearable
      {rules}
      bind:validation={validationArray[1]}
      bind:value={courseName}>
      Course Name
    </TextField>
  </Row>
  <Row>
    <Col>
      <TextField
        clearable
        {rules}
        bind:validation={validationArray[2]}
        bind:value={credits}>
        Credits
      </TextField>
    </Col>
    <Col>
      <Select bind:value={subject} items={subjects} mandatory={true}>
        Subject
      </Select>
    </Col>
    <Col>
      <Select bind:value={GERCategory} items={GERCategories}>
        GER Category (if any)
      </Select>
    </Col>
  </Row>
  <Row style="padding:20px;">
    <Textarea autogrow {rules} counter={1000} bind:value={courseDesc}>
      Course Description
    </Textarea>
  </Row>
  {#if validationError}
    <Row noGutters>
      <Button class="red white-text" on:click={addCourse}>Add Course</Button>
    </Row>
    <div style="color:red;">Please fill in all required fields.</div>
  {:else}
    <Row noGutters>
      <Button class="green white-text" on:click={addCourse}>Add Course</Button>
    </Row>
  {/if}
</div>
