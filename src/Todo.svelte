<script>
  import { ListItem, Checkbox, Divider } from "svelte-materialify";
  import firebase from "@firebase/app";
  export let todo;
  export let userDocRef;
  function removeTodo(task) {
    userDocRef.update(
      {
        todos: firebase.firestore.FieldValue.arrayRemove(task)
      },
      { merge: true }
    );
  }
</script>

<ListItem>
  <div slot="prepend">
    <Checkbox on:change={removeTodo(todo)} />
  </div>
  {todo.task}
  <span slot="subtitle">
    <div style="display: flex; flex-direction:column;">
      <div>{todo.description}</div>
      <div style="align-self: flex-end">{todo.date}</div>
    </div>
  </span>

</ListItem>
<Divider />
