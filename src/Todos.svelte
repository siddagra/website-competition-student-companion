<script>
  import Todo from "./Todo.svelte";
  import firebase from "@firebase/app";
  import Datepicker from "svelte-calendar";
  import {
    Row,
    Col,
    TextField,
    Button,
    Textarea,
    List,
    Subheader
  } from "svelte-materialify";
  import { fade, fly } from "svelte/transition";

  export let userDocRef;
  export let userDoc;
  let task = "";
  let date = "";
  let description = "";

  function addTodo(todoText) {
    if (task.length > 0)
      userDocRef.update(
        {
          todos: firebase.firestore.FieldValue.arrayUnion({
            task: task,
            status: "pending",
            date: date,
            description: description,
            completed: false,
            archived: false,
            timeCreated: firebase.firestore.Timestamp.fromDate(new Date()),
            timeCompleted: null
          })
        },
        { merge: true }
      );
    //close new task window by emptying "add new task" field
    task = "";
  }
</script>

<div style="min-height:1000px">
  <List>
    <Subheader>To-Do List</Subheader>
    {#each userDoc.todos as todo}
      <Todo {todo} {userDocRef} />
    {/each}
  </List>

  <TextField
    bind:value={task}
    on:keypress={e => {
      if (e.key == 'Enter') addTodo();
    }}>
    Add a new task
  </TextField>
  {#if task.length > 0}
    <div style="padding: 20px; padding-bottom:300px; overflow:hidden; ">
      <div
        in:fly={{ y: -200, duration: 200 }}
        out:fade
        style="padding-top:10px;">
        <Row noGutters={true}>
          <Textarea bind:value={description}>Add description</Textarea>
        </Row>
        <Row noGutters={true}>
          <Col />
          <Col style="align-self-end">
            <Datepicker bind:formattedSelected={date} />
          </Col>
        </Row>
        <Row noGutters={true}>
          <Button on:click={addTodo}>add</Button>
        </Row>
      </div>
    </div>
  {/if}
</div>
