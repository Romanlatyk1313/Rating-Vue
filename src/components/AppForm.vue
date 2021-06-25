<template>
  <div>
    <form class="form" method="post" @submit.prevent="openPopup">
      <h1>FEEDBACK FORM</h1>
      <div>
        <input
          class="form__input"
          type="text"
          placeholder="Name"
          v-model.trim="form.name"
        />
      </div>
      <div class="form__error">{{ errorEmail }}</div>
      <div>
        <input
          class="form__input"
          type="email"
          placeholder="Email"
          @mouseout="validateEmail"
          v-model.trim="form.email"
        />
      </div>
      <app-rating @changeValue="changeValue"></app-rating>
      <div>
        <textarea placeholder="Comment" v-model="form.comment"></textarea>
      </div>
      <div>
        <button class="form__button" type="submit">Send Feedback</button>
      </div>
    </form>

    <app-popup
      :is-open="isPopupOpen"
      @ok="popupConfirmed"
      @close="isPopupOpen = false"
    >
      <div class="popup-wrong">{{ wrong }}</div>

      <template #actions="{ confirm }">
        <button class="popup-button" @click="confirm">OK</button>
      </template>
    </app-popup>
  </div>
</template>

<script>
import Rating from "./AppRating.vue";
import Popup from "./AppPopup.vue";
export default {
  name: "Form",
  components: {
    "app-rating": Rating,
    "app-popup": Popup,
  },

  data() {
    return {
      buttonDisabled: false,
      errorEmail: "",
      wrong: "",
      isPopupOpen: false,
      confirmation: "",
      form: {
        name: "",
        email: "",
        rating: "",
        comment: "",
      },
      errors: [],
    };
  },
  methods: {
    validateEmail() {
      let reg = /^[A-Z0-9._%+-]+@[A-Z0-9-]+.+.[A-Z]{2,4}$/i;
      if (reg.test(this.form.email) === false) {
        this.errorEmail = "Please paste correct email address";
      } else if (reg.test(this.form.email) === true) {
        this.errorEmail = "";
      }
    },
    openPopup() {
      if (
        (this.form.name !== "") &
        (this.form.email !== "") &
        (this.form.rating !== "") &
        (this.form.comment !== "")
      ) {
        this.formSubmit();
        this.confirmation = "";
        this.isPopupOpen = true;
      }
    },

    popupConfirmed() {
      this.isPopupOpen = false;
    },
    changeValue(data) {
      this.form.rating = data;
    },
    async formSubmit() {
      try {
        const res = await fetch(
          "https://it-academy-viseven.herokuapp.com/task6-check",
          {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },

            body: JSON.stringify({
              name: this.form.name,
              email: this.form.email,
              rating: this.form.rating,
              comment: this.form.comment,
            }),
          }
        );
        const data = await res.json();
        if (res.status === 200 || res.status === 201) {
          this.wrong = "Thank you";
        } else {
          this.errors = data;
        }
      } catch (error) {
        console.error(error);
        this.wrong = "Someting went wrong";
      }
    },
  },
};
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.form__input,
textarea {
  width: 400px;
  height: 40px;
  font-size: 28px;
  color: black;
}
.form__error {
  display: flex;
  align-items: center;
  height: 60px;
  color: #ed2525;
  font-size: 24px;
}

textarea {
  height: 200px;
  margin-top: 50px;
}
.form__button,
.popup-button {
  height: 40px;
  font-size: 28px;
  background-color: aqua;
  border-radius: 10%;
}
.popup-button {
  display: block;
  margin-bottom: 20px;
}
.popup-wrong {
  font-size: 32px;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 40px;
  margin-bottom: 100px;
}
</style>