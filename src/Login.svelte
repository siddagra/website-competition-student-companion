<script>
  import App from "./App.svelte";
  import Head from "./Head.svelte";
  import BackgroundEffects from "./BackgroundEffects.svelte";

  //firebase
  import { FirebaseApp, Doc, Collection, User } from "sveltefire";
  import firebase from "@firebase/app";
  import "@firebase/firestore";
  import "@firebase/auth";
  import "@firebase/storage";
  import "@firebase/performance"; // Optional
  import "@firebase/analytics"; // Optional

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
  const facebookProvider = new firebase.auth.FacebookAuthProvider();
  const GithubProvider = new firebase.auth.GithubAuthProvider();
  const microsoftProvider = new firebase.auth.OAuthProvider("microsoft.com");
  let email, password;
  const db = firebase.firestore();
</script>

<style>
  .container {
    position: absolute;
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
  }

  form {
    background: rgba(255, 255, 255, 0.3);
    padding: 3em;
    height: fit-content;
    width: 500px;
    border-radius: 20px;
    border-left: 1px solid rgba(255, 255, 255, 0.3);
    border-top: 1px solid rgba(255, 255, 255, 0.3);
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    box-shadow: 20px 20px 40px -6px rgba(0, 0, 0, 0.2);
    text-align: center;
    position: relative;
    transition: all 0.2s ease-in-out;
  }
  form p {
    font-weight: 500;
    color: #fff;
    opacity: 0.7;
    font-size: 1.4rem;
    margin-top: 0;
    margin-bottom: 60px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  }
  form a {
    text-decoration: none;
    color: #ddd;
    font-size: 12px;
    cursor: pointer;
  }
  form a:hover {
    text-shadow: 2px 2px 6px #00000040;
  }
  form a:active {
    text-shadow: none;
  }
  form input {
    background: transparent;
    width: 200px;
    padding: 1em;
    margin-bottom: 2em;
    border: none;
    border-left: 1px solid rgba(255, 255, 255, 0.3);
    border-top: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 5000px;
    -webkit-backdrop-filter: blur(5px);
    backdrop-filter: blur(5px);
    box-shadow: 4px 4px 60px rgba(0, 0, 0, 0.2);
    color: #fff;
    font-family: Montserrat, sans-serif;
    font-weight: 500;
    transition: all 0.2s ease-in-out;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  }
  form input:hover {
    background: rgba(255, 255, 255, 0.1);
    box-shadow: 4px 4px 60px 8px rgba(0, 0, 0, 0.2);
  }
  form input[type="email"]:focus,
  form input[type="password"]:focus {
    background: rgba(255, 255, 255, 0.1);
    box-shadow: 4px 4px 60px 8px rgba(0, 0, 0, 0.2);
  }
  form input[type="button"] {
    margin-top: 10px;
    width: 150px;
    font-size: 1rem;
  }
  form input[type="button"]:hover {
    cursor: pointer;
  }
  form input[type="button"]:active {
    background: rgba(255, 255, 255, 0.2);
  }
  form:hover {
    margin: 4px;
  }

  ::-moz-placeholder {
    font-family: Montserrat, sans-serif;
    font-weight: 400;
    color: #fff;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
  }

  :-ms-input-placeholder {
    font-family: Montserrat, sans-serif;
    font-weight: 400;
    color: #fff;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
  }

  ::placeholder {
    font-family: Montserrat, sans-serif;
    font-weight: 400;
    color: #fff;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
  }

  .drop {
    background: rgba(255, 255, 255, 0.3);
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    border-radius: 10px;
    border-left: 1px solid rgba(255, 255, 255, 0.3);
    border-top: 1px solid rgba(255, 255, 255, 0.3);
    box-shadow: 10px 10px 60px -8px rgba(0, 0, 0, 0.2);
    position: absolute;
    transition: all 0.2s ease;
  }
  .drop-1 {
    height: 80px;
    width: 80px;
    top: -20px;
    left: -40px;
    z-index: -1;
  }
  .drop-2 {
    height: 80px;
    width: 80px;
    bottom: -30px;
    right: -10px;
  }
  .drop-3 {
    height: 100px;
    width: 100px;
    bottom: 120px;
    right: -50px;
    z-index: -1;
  }
  .drop-4 {
    height: 120px;
    width: 120px;
    top: -60px;
    right: -60px;
  }
  .drop-5 {
    height: 60px;
    width: 60px;
    bottom: 170px;
    left: 90px;
    z-index: -1;
  }

  a,
  input:focus,
  select:focus,
  textarea:focus,
  button:focus {
    outline: none;
  }

  * {
    padding: 0px;
    margin: 0px;
  }

  ul {
    position: relative;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
    grid-row-gap: 20px;
    grid-template-rows: repeat(auto);
    grid-auto-flow: row;
    transform-style: preserve-3d;
  }

  ul li {
    position: relative;
    list-style: none;
    width: 60px;
    height: 60px;
    margin: 0px 20px;
  }

  ul li:before {
    content: "";
    position: absolute;
    bottom: -10px;
    left: -5px;
    width: 100%;
    height: 10px;
    background: #2a2a2a;
    trnasform-origin: top;
    transform: skewX(-41deg);
  }

  ul li:after {
    content: "";
    position: absolute;
    top: 5px;
    left: -9px;
    width: 9px;
    height: 100%;
    background: #2a2a2a;
    trnasform-origin: right;
    transform: skewY(-49deg);
  }

  ul li span {
    position: absolute;
    top: 0;
    lef: 0;
    width: 100%;
    height: 100%;
    display: flex !important;
    background: rgba(0, 0, 0, 0.02);
    justify-content: center;
    align-items: center;
    color: #fff;
    font-size: 30px !important;
    transition: 1.5s ease-out;
  }

  ul li:hover span {
    z-index: 1000;
    transition: 0.3s;
    color: #fff;
    box-shadw: -1px 1px 1px rgba(0, 0, 0, 0.5);
  }

  ul li:hover span:nth-child(5) {
    transform: translate(40px, -40px);
    opacity: 1;
  }

  ul li:hover span:nth-child(4) {
    transform: translate(30px, -30px);
    opacity: 0.8;
  }

  ul li:hover span:nth-child(3) {
    transform: translate(20px, -20px);
    opacity: 0.6;
  }

  ul li:hover span:nth-child(2) {
    transform: translate(10px, -10px);
    opacity: 0.4;
  }

  ul li:hover span:nth-child(1) {
    transform: translate(0px, 0px);
    opacity: 0.2;
  }

  ul li:nth-child(1):hover span {
    background: #4267b2 !important;
  }

  ul li:nth-child(2):hover span {
    background: #d44638 !important;
  }

  ul li:nth-child(3):hover span {
    background: #171515 !important;
  }

  ul li:nth-child(4):hover span {
    background: #fceb00 !important;
  }
  @media only screen and (max-width: 800px) {
    .container {
      width: 80%;
    }
    form {
      width: 100%;
    }
    .drop {
      visibility: hidden;
    }
  }
  @media only screen and (max-width: 625px) {
    .container {
      width: 120%;
    }
    form {
      width: 100%;
    }
  }
</style>

<svelte:head>

  <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500&display=swap"
    rel="stylesheet" />
  <style>
    @import url("//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css");
  </style>
</svelte:head>

<Head />
<BackgroundEffects />

<FirebaseApp {firebase}>
  <User let:user let:auth>
    <App {user} {db} {auth} />

    <div slot="signed-out">
      <div class="container">
        <form>
          <p>Welcome</p>
          <input type="email" placeholder="Email" bind:value={email} />
          <br />
          <input type="password" placeholder="Password" bind:value={password} />
          <br />
          <input
            type="button"
            value="Sign in"
            on:click={firebase
              .auth()
              .createUserWithEmailAndPassword(email, password)} />
          <br />
          <div>
            <ul>
              <li>
                <a on:click={() => auth.signInWithPopup(facebookProvider)}>
                  <span />
                  <span />
                  <span />
                  <span />
                  <span class="fa fa-facebook" />
                </a>
              </li>
              <li>
                <a on:click={() => auth.signInWithPopup(googleProvider)}>
                  <span />
                  <span />
                  <span />
                  <span />
                  <span class="fa fa-google" />
                </a>
              </li>
              <li>
                <a on:click={() => auth.signInWithPopup(GithubProvider)}>
                  <span />
                  <span />
                  <span />
                  <span />
                  <span class="fa fa-github" />
                </a>
              </li>
              <li>
                <a on:click={() => auth.signInWithPopup(microsoftProvider)}>
                  <span />
                  <span />
                  <span />
                  <span />
                  <span class="fa fa-windows" />
                </a>
              </li>
            </ul>
          </div>
          <br />

          <a on:click={() => firebase.auth().signInAnonymously()}>
            Sign in as Guest
          </a>
        </form>

        <div class="drops">
          <div class="drop drop-1" />
          <div class="drop drop-2" />
          <div class="drop drop-3" />
          <div class="drop drop-4" />
          <div class="drop drop-5" />
        </div>
      </div>
    </div>
  </User>
</FirebaseApp>
