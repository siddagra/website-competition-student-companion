<script>
  import ProgressTimeline from "./ProgressTimeline.svelte";
  import { userCourses, GERCategories } from "./stores.js";
  let major = "Computer Science";
  let majorCourses, freeElectives, GERCourses;

  //$: reactive variables that change dynamically
  $: majorCourses = $userCourses.filter(course => course.subject == major);
  $: freeElectives = $userCourses.filter(
    course =>
      course.subject != major && !GERCategories.includes(course.GERCategory)
  );
  $: GERCourses = $userCourses.filter(course =>
    GERCategories.includes(course.GERCategory)
  );
</script>

<style lang="scss">
  .mainContainer {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    grid-template-rows: repeat(auto);
    grid-auto-flow: row;
  }
</style>

<!--
<svg
  width="497"
  height="219"
  viewBox="0 0 497 219"
  fill="none"
  xmlns="http://www.w3.org/2000/svg">
  <path
    fill-rule="evenodd"
    clip-rule="evenodd"
    d="M-38.5 196C-38.5 196 34 91 133.5 91C233 91 427 159 532.5 30C638 -99 518
    236 518 236L-49 246.5L-38.5 196Z"
    fill="#FF768E" />
</svg>-->

<div class="mainContainer">
  <ProgressTimeline
    coursesType="Major"
    courses={majorCourses}
    totalCreditsReq={60} />
  <ProgressTimeline
    coursesType="Free Electives"
    courses={freeElectives}
    totalCreditsReq={18} />
  <ProgressTimeline
    coursesType="GER"
    courses={GERCourses}
    totalCreditsReq={30} />
</div>
