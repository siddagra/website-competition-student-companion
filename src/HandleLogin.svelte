<script>
  import LoadCompo from "./LoadCompo.svelte";
  import { userDoc, db } from "./stores.js";
  import { onMount } from "svelte";
  export let user;
  import firebase from "@firebase/app";
  import "@firebase/database";
  let value;
  db.collection("users")
    .doc(user.uid)
    .onSnapshot(doc => {
      userDoc.set(doc.data());
    });
</script>

<LoadCompo />
