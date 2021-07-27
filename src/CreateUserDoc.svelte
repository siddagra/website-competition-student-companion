<script>
  export let userDocRef;
  export let user;
  userDocRef.set({
    uid: user.uid,
    email: user.email,
    courses: [],
    todos: []
  });
</script>
