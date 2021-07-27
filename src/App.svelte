<script>
  import CreateUserDoc from "./CreateUserDoc.svelte";
  import BackgroundEffects from "./BackgroundEffects.svelte";
  import Progress from "./Progress.svelte";
  import Admin from "./Admin.svelte";
  import ListCourses from "./ListCourses.svelte";
  import CoursesAccordians from "./CoursesAccordians.svelte";
  import Todos from "./Todos.svelte";

  //frontend UI libraries
  import {
    MaterialApp,
    Button,
    Tabs,
    Tab,
    Window,
    WindowItem,
    AppBar
  } from "svelte-materialify";

  //firebase
  import { Doc, Collection } from "sveltefire";
  export let user;
  export let db;
  export let auth;

  //theme for svelte-materialify
  let theme = "light";
  //change theme function
  function toggleTheme() {
    if (theme === "light") theme = "dark";
    else theme = "light";
  }

  let value = 0;
</script>

<style>
  .app {
    margin-left: 20%;
    margin-right: 20%;
    height: fit-content;
    overflow: hidden;
  }
  .container {
    min-height: 1000px;
    padding-right: 30px;
  }
</style>

<BackgroundEffects />
<div class="app">
  <MaterialApp {theme}>

    <AppBar class="deep-purple white-text">
      <span slot="title">Title</span>

      <div style="flex-grow:1" />
      <!--change theme button-->
      <Button class="deep-purple-text" on:click={toggleTheme}>
        {theme == 'light' ? 'Dark mode' : 'Light mode'}
      </Button>
      <Button class="deep-purple-text" on:click={() => auth.signOut()}>
        Sign Out
      </Button>
      <div slot="extension">
        <Tabs class="deep-purple-text" bind:value fixedTabs>
          <div slot="tabs">
            <Tab>Admin</Tab>
            <Tab>Courses</Tab>
            <Tab>Progress</Tab>
            <Tab>To-do List</Tab>
          </div>
        </Tabs>
      </div>
    </AppBar>
    <div class="container">

      <Collection
        path={db.collection('courses')}
        let:data={courses}
        let:ref={coursesRef}>

        <Window {value} class="ma-4">
          <!-- 3. 📜 Get a Firestore document owned by a user -->
          <Doc
            path={`users/${user.uid}`}
            let:data={userDoc}
            let:ref={userDocRef}>
            <span slot="loading">Loading user...</span>
            <span slot="fallback">
              <CreateUserDoc {userDocRef} {user} />
            </span>

            <WindowItem>
              <h3>Courses</h3>
              <ListCourses
                userCoursesCodes={userDoc.courses}
                {userDocRef}
                {courses}
                {user} />
              <Collection
                path={db.collection('ratings')}
                let:data={ratingsColl}>
                <h3>Available Courses</h3>
                <CoursesAccordians {courses} {userDocRef} {ratingsColl} />
              </Collection>
            </WindowItem>

            <WindowItem>
              <Progress userCoursesCodes={userDoc.courses} {courses} />
            </WindowItem>
            <WindowItem>
              <Todos {userDocRef} {userDoc} />
            </WindowItem>
          </Doc>
          <WindowItem>
            <Admin {coursesRef} />
          </WindowItem>
        </Window>
      </Collection>
    </div>
  </MaterialApp>
</div>
