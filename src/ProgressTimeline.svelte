<script>
  export let coursesType, courses, totalCreditsReq;
  //get sum of property "credits" in all courses
  let creditsDone = courses.reduce((sum, { credits }) => sum + credits, 0);
  let creditsLeft = totalCreditsReq - creditsDone;
</script>

<style>
  .cardTop {
    width: 497px;
    height: 240px;
    background: #fac24f;
    border-radius: 20px;
    display: flex;
    overflow: hidden;
    align-items: flex-end;
  }

  .timeline {
    width: 355px;
    /*position: absolute;
    top: 20px;
    left: 71px;*/
    margin-bottom: 20px;
    height: fit-content;
    background: #ecf1f524;
    mix-blend-mode: normal;
    backdrop-filter: blur(15px);
    overflow: hidden;
    box-shadow: 0px 20px 53px -30px rgba(95, 102, 173, 0.566816);
    border-radius: 10px;
  }

  .timeline h2 {
    text-transform: uppercase;
    font-size: 3em;
  }

  .timeline .subheading {
    line-height: 22px;
    /* identical to box height */
    margin-left: 66px;
    /*color: #ffffff;*/
  }

  .timeline .box {
    width: 100%;
    margin-top: 99.5px;
  }

  .timeline .box .container {
    width: 100%;
    display: flex;
    padding-bottom: 60px;
  }

  .timeline .box .container .lines {
    margin-left: 40px;
    margin-top: 6px;
  }

  .timeline .box .container .lines .dot {
    width: 14px;
    height: 14px;
    background: #d1d6e6;
    border-radius: 7px;
  }

  .timeline .box .container .lines .line {
    height: 20px;
    width: 4px;
    background: #d1d6e6;
    margin-left: 5.3px;
  }

  .timeline .box .container .card {
    width: 249px;
    padding: 5px;
    text-transform: capitalize;
    border-radius: 10px;
    margin-bottom: 10px;
    /*for light mode*/
    box-shadow: 0 2px 2px 0 #c7c7c740;
    /*for dark mode*/
    box-shadow: 0px 16px 15px -10px rgba(216, 216, 216, 0.431);
  }

  .timeline .box .container .card.mid {
    height: 71px;
  }
</style>

<div class="timeline">
  <h2 class>{coursesType}</h2>
  <div class="subheading">Credits: {creditsDone}/{totalCreditsReq}</div>
  <div class="box">
    <div class="container">
      <div class="lines">
        {#each courses as course}
          <div style="display:flex; flex-direction:row;position:relative;">
            <div style="display:flex; flex-direction:column;">
              {#each Array(course.credits * 2) as credit}
                <div class="line green" />
              {/each}
              <div class="dot green" />
            </div>
            <div
              class="card"
              style="position:absolute; width:250px; left:20px; bottom:-40px;">
              <h6 class="position:absolute;">{course.code}:</h6>
              {course.name}
            </div>
          </div>
        {/each}
        <!--add a red line if all courses in the category are not complete-->
        {#if creditsLeft > 0}
          {#each Array(creditsLeft * 2) as halfCredit}
            <div class="line red" />
          {/each}
          <div class="dot red" />
        {/if}
      </div>
    </div>
  </div>
</div>
