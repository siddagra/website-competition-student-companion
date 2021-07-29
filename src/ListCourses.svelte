<script>
  import ListCourse from "./ListCourse.svelte";
  import { userCourses } from "./stores.js";
  import { ExpansionPanels } from "svelte-materialify";

  export let userCoursesCodes;
  export let courses;
  export let user;
  export let userDocRef;

  //change value of $userCourses from stores.js
  $: userCourses.set(
    courses.filter(course => userCoursesCodes.includes(course.code))
  );
</script>

<ExpansionPanels accordion>
  {#each $userCourses as userCourse}
    <ListCourse {userCourse} {user} {userDocRef} />
  {:else}No courses added yet...{/each}
</ExpansionPanels>
