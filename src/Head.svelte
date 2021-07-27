<svelte:head>
  <style>
    /* Scroll 4 */
    ::-webkit-scrollbar {
      width: 12px;
      height: 12px;
    }

    ::-webkit-scrollbar-track {
      border-radius: 10px;
      background-color: rgba(0, 0, 0, 0.4);
    }

    ::-webkit-scrollbar-thumb {
      background-color: #9966ff;
      background-image: -webkit-linear-gradient(
        45deg,
        rgba(255, 255, 255, 0.3) 20%,
        transparent 20%,
        transparent 40%,
        rgba(255, 255, 255, 0.3) 40%,
        rgba(255, 255, 255, 0.3) 60%,
        transparent 60%,
        transparent 80%,
        rgba(255, 255, 255, 0.3) 80%
      );
      border-radius: 10px;
    }

    ::-webkit-scrollbar {
      width: 15px;
      height: 15px;
    }

    ::-webkit-scrollbar-track {
      border-radius: 10px;
      background-color: rgba(255, 255, 255, 0.1);
    }

    ::-webkit-scrollbar-thumb {
      background-image: linear-gradient(45deg, #00aeff, #a68eff);
      border-radius: 10px;
      -webkit-box-shadow: rgba(0, 0, 0, 0.12) 0 3px 13px 1px;
    }

    body {
      background: #8e2de2;
      /* fallback for old browsers */
      background: -webkit-linear-gradient(45deg, #8e2de2, #65a6fa);
      /* Chrome 10-25, Safari 5.1-6 */
      background: linear-gradient(45deg, #8e2de2, #65a6fa);
      /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
      background-position: center;
      /* Center the image */
      background-repeat: no-repeat;
      /* Do not repeat the image */
      background-size: cover;
      /* Resize the background image to cover the entire container */
      background-attachment: fixed;
      min-height: 100vh;
      height: fit-content;
      position: relative;
    }
  </style>
  <link
    href="https://fonts.googleapis.com/icon?family=Material+Icons"
    rel="stylesheet" />
</svelte:head>
