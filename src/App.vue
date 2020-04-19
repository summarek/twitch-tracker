<template>
  <div class="container">
    <header>
      <h1>Twitch chat log viewer!</h1>
    </header>
    <main>
      <div class="messages">
        <div
          class="message"
          v-for="message in messagesArray"
          :key="message.time"
        >
          <span class="messageText">
            <span class="date">{{ message.time }} </span>
            <span class="author">{{ message.twitchAuthor }} </span>
            {{ message.twitchMessage }}
          </span>
        </div>
      </div>
      <div class="inputs">
        <div class="form__group field">
          <input
            type="input"
            class="form__field"
            placeholder="Twitch nickname"
            name="twitchName"
            v-model.trim="nicknameTwitch"
            id="name"
            required
          />
          <label for="name" class="form__label">Twitch Nickname</label>
        </div>
        <div class="form__group field">
          <input
            type="input"
            class="form__field"
            placeholder="Twitch Channel"
            name="twitchChannel"
            v-model.trim="channelTwitch"
            id="name"
            required
          />
          <label for="name" class="form__label">Twitch Channel</label>
        </div>
        <div class="btn" @click="getData(channelTwitch, nicknameTwitch)">
          Szukaj!
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      messagesArray: [],
      nicknameTwitch: "",
      channelTwitch: "",
    };
  },
  methods: {
    getData(channel, nick) {
      axios
        .get(`http://localhost:3000/${channel}/${nick}`)
        .then((response) => {
          // handle success
          console.log(response.data);
          this.messagesArray = response.data;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Roboto+Mono&display=swap");
* {
  padding: 0;
  margin: 0;
  font-family: Helvetica, sans-serif;
  font-weight: 300;
}
body {
  width: 100%;
  height: 100vh;
  background-color: #9147ff;
}
.container {
  height: 100vh;
}
header {
  padding-top: 70px;
  padding-bottom: 70px;
  display: flex;
  justify-content: center;
  align-content: center;

  h1 {
    color: white;
    text-align: center;
  }
}
main {
  margin-top: 50px;
  width: 70vw;
  height: 70%;
  margin: 0 auto;
  background-color: rgb(255, 255, 255);
  border-radius: 10px;
  .messages {
    border-radius: 10px;
    height: 90%;
    overflow-y: scroll;
    background-color: rgb(255, 255, 255);
    .message {
      min-height: 8%;
      max-width: 100%;
      padding: 0.7rem;
      .messageText {
        max-width: 100%;
        word-break: break-all;
        font-weight: 200;
        font-size: 1.2rem;
        .date {
          font-size: 0.9rem;
        }
        .author {
          font-weight: bold;
          font-size: 1.22rem;
          color: blue;
        }
      }
    }
  }
  .message:nth-child(odd) {
    background-color: rgb(240, 240, 240);
  }
  .inputs {
    padding: 8px;
    padding-left: 14px;
    display: flex;
    justify-content: flex-start;
    background-color: #222225;
    height: 10%;
    border-radius: 0 0 10px 10px;
    .btn {
      cursor: pointer;
      margin: 0 auto;
      font-family: Helvetica, sans-serif;
      font-size: 1.2rem;
      font-weight: bold;
      padding: 12px 25px 12px 25px;
      border-radius: 5px;
      justify-self: center;
      align-self: center;
      background-color: #9f5fff;
      color: white;
    }
  }
}
//FORM SCSS
$primary: #9147ff;
$secondary: #9147ff;
$white: rgb(255, 255, 255);
$gray: #dadada;
.form__group {
  position: relative;
  padding: 10px;
  margin-top: 5px;
  width: 30%;
}

.form__field {
  font-family: inherit;
  width: 100%;
  border: 0;
  border-bottom: 2px solid $gray;
  outline: 0;
  font-size: 1.3rem;
  color: $white;
  padding: 7px 0;
  background: transparent;
  transition: border-color 0.2s;

  &::placeholder {
    color: transparent;
  }

  &:placeholder-shown ~ .form__label {
    font-size: 1.3rem;
    cursor: text;
    top: 20px;
  }
}

.form__label {
  position: absolute;
  top: 0;
  display: block;
  transition: 0.2s;
  font-size: 1rem;
  color: $gray;
}

.form__field:focus {
  ~ .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: $primary;
    font-weight: 700;
  }
  padding-bottom: 6px;
  font-weight: 700;
  border-width: 3px;
  border-image: linear-gradient(to right, $primary, $secondary);
  border-image-slice: 1;
}
/* reset input */
.form__field {
  &:required,
  &:invalid {
    box-shadow: none;
  }
}
</style>
