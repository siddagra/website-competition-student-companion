<script>
  import CourseAccordian from "./CourseAccordian.svelte";
  import { userCourses } from "./stores.js";
  import { ExpansionPanels } from "svelte-materialify";

  export let userCoursesCodes;
  export let courses;
  export let user;
  export let userDocRef;

  $: userCourses.set(
    courses.filter(course => userCoursesCodes.includes(course.code))
  );

  $: console.log($userCourses);
</script>

<ExpansionPanels accordion>
  {#each $userCourses as userCourse}
    <CourseAccordian {userCourse} {user} {userDocRef} />
  {:else}No courses added yet...{/each}
</ExpansionPanels>
