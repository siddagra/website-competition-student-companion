<script>
  import { GERCategories } from "./stores.js";
  import {
    ExpansionPanel,
    ExpansionPanels,
    Button,
    Row,
    Col
  } from "svelte-materialify";
  import Rate from "svelte-rate-it/Rate.svelte";
  import firebase from "@firebase/app";

  export let userCourse;
  export let user;
  export let userDocRef;

  //function to add a course to
  function removeCourse(courseCode) {
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
  }
</script>

<style>
  /*container for expansion panel content from svelte-materialify library
content needs to be flex-direction:column and not row*/
  .s-expansion-panel__content {
    flex-direction: column;
  }
</style>

<ExpansionPanel>
  <span slot="header">{userCourse.code}: {userCourse.name}</span>
  <div style="display:flex; flex-direction: column; width:100%">
    <Row>
      <Col>
        <div>{userCourse.description}</div>
      </Col>
    </Row>
    <!--additional information-->
    <!--
  <ExpansionPanels accordion>
    <ExpansionPanel>
      <span slot="header">Professors</span>
      <div style="display:flex; flex-direction:column">
        {#each userCourse.professors as professor}
          <li>{professor}</li>
        {:else}No professors assigned...{/each}
      </div>
    </ExpansionPanel>
  </ExpansionPanels>-->
    <Row>
      {#if userCourse.GERCategory}
        <Col>GERCategory: {userCourse.GERCategory}</Col>
      {/if}
    </Row>
    <Row>
      <Col>Credits: {userCourse.credits}</Col>
    </Row>
    <Row>
      <Col>
        <Button on:click={removeCourse(userCourse.code)}>Remove</Button>
      </Col>
      <Col style="align-self: flex-end;">
        <Rate {afterRate} />
      </Col>
    </Row>
  </div>
</ExpansionPanel>
