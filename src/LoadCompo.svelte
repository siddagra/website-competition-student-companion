<script>
  import ListCourses from "./ListCourses.svelte";
  import CoursesAccordians from "./CoursesAccordians.svelte";

  //frontend UI libraries
  import "smelte/src/tailwind.css";
  import { MaterialApp, Button } from "svelte-materialify";

  //firebase libraries
  import {
    FirebaseApp,
    Doc,
    Collection,
    User,
    UploadTask,
    StorageRef
  } from "sveltefire";
  import firebase from "firebase/app";
  import "firebase/firestore";
  import "firebase/auth";
  import "firebase/storage";
  import "firebase/performance"; // Optional
  import "firebase/analytics"; // Optional

  //firebase api config
  const config = {
    apiKey: "AIzaSyDo0CdNNtkGlzDE_6r8Sc6QyzHyHEFzOFE",
    authDomain: "website-competition.firebaseapp.com",
    projectId: "website-competition",
    storageBucket: "website-competition.appspot.com",
    messagingSenderId: "742534445003",
    appId: "1:742534445003:web:43da1acf463532b3c9684e",
    measurementId: "G-2HD1J43NPJ"
  };
  //initialise firebase app, database, auth and googleAuthProvider
  firebase.initializeApp(config);
  const auth = firebase.auth();
  const googleProvider = new firebase.auth.GoogleAuthProvider();
  const db = firebase.firestore();

  //theme for svelte-materialify
  let theme = "light";
  //change theme function
  function toggleTheme() {
    if (theme === "light") theme = "dark";
    else theme = "light";
  }
</script>

<svelte:head>
  <link
    href="https://fonts.googleapis.com/icon?family=Material+Icons"
    rel="stylesheet" />
</svelte:head>

<FirebaseApp {firebase}>
  <MaterialApp {theme}>
    <!--change theme button-->
    <Button on:click={toggleTheme}>
      {theme == 'light' ? 'Dark mode' : 'Light mode'}
    </Button>

    <User let:user let:auth>

      <p>Howdy, {user.uid}</p>

      <Button on:click={() => auth.signOut()}>Sign Out</Button>

      <div slot="signed-out">
        <Button on:click={() => auth.signInWithPopup(googleProvider)}>
          Sign in using GMail
        </Button>
      </div>

      <!-- 3. 📜 Get a Firestore document owned by a user -->
      <Doc
        path={`userCourses/${user.uid}`}
        let:data={userCourse}
        let:ref={userCourseRef}
        log>

        <h2>{userCourse.title}</h2>

        <p>
          Document created at
          <em>{new Date(userCourse.createdAt).toLocaleString()}</em>
        </p>

        <span slot="loading">Loading userCourse...</span>
        <span slot="fallback">
          <button
            on:click={() => userCourseRef.set({
                title: '📜 I like Svelte',
                createdAt: Date.now()
              })}>
            Create Document
          </button>
        </span>

        <!-- 4. 💬 Get all the comments in its subcollection -->

        <h3>Comments</h3>
        <Collection
          path={userCourseRef.collection('comments')}
          query={ref => ref.orderBy('createdAt')}
          let:data={comments}
          let:ref={commentsRef}
          log>

          {#if !comments.length}No comments yet...{/if}

          {#each comments as comment}
            <p>
              <!-- ID: <em>{comment.ref.id}</em> -->
            </p>
            <p>
              {comment.text}
              <button on:click={() => comment.ref.delete()}>Delete</button>
            </p>
          {/each}

          <button
            on:click={() => commentsRef.add({
                text: '💬 Me too!',
                createdAt: Date.now()
              })}>
            Add Comment
          </button>

          <span slot="loading">Loading comments...</span>

        </Collection>
      </Doc>

    </User>
    <Collection path={db.collection('courses')} let:data={courses}>
      <CoursesAccordians {courses} />
    </Collection>
    <ListCourses />
  </MaterialApp>
</FirebaseApp>
