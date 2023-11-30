<template>
  <div class="d-flex justify-content-center container mt-5">
    <div class="wrapper">
      <div class="main scroll-wrapper">
        <div class="px-2 scroll">
          <div class="d-flex align-items-center">
            <div class="text-left pr-1"></div>
            <div class="pr-2 pl-1">
              <div v-for="message in chatMessages" :key="message.id">
                <div v-if="message.type === 'bot'">
                  <div class="d-flex align-items-center text-left">
                    <div class="pr-1">
                      <img
                        src="../../public/cat.jpeg"
                        width="30"
                        class="img1"
                      />
                    </div>
                    <div>
                      <span class="name">Bot</span>
                      <p class="msg">
                        {{ message.text }}
                      </p>
                    </div>
                  </div>
                </div>
                <div v-else>
                  <div
                    class="d-flex align-items-center text-right justify-content-end"
                  >
                    <div class="pr-2">
                      <span class="name">User</span>
                      <p class="msg">{{ message.text }}</p>
                    </div>
                    <div>
                      <img
                        src="../../public/panda.jpeg"
                        width="30"
                        class="img1"
                      />
                    </div>
                  </div>
                </div>
              </div>
              <div v-if="showOptions" class="flex-container">
                <p>Выберите вариант:</p>
                <button
                  @click="handleUserResponse('Заказать пиццу 🍕')"
                  class="option"
                >
                  Заказать пиццу 🍕
                </button>
                <button
                  @click="handleUserResponse('Установить будильник ⏰')"
                  class="option"
                >
                  Установить будильник ⏰
                </button>
                <button
                  @click="handleUserResponse('Вывести погоду ⛅')"
                  class="option"
                >
                  Вывести погоду ⛅
                </button>
              </div>

              <input
                type="text"
                name="text"
                class="form-control"
                placeholder="Напишите..."
                v-model="userMessage"
                @keyup.enter="handleUserResponse(userMessage)"
              />
              <div
                class="icondiv d-flex justify-content-end align-content-center text-center ml-2"
              >
                <i class="fa fa-paperclip icon1"></i>
                <i class="fa fa-arrow-circle-right icon2"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      chatMessages: [
        { id: 1, text: "Привет! Что я могу для Вас сделать?", type: "bot" },
      ],
      userMessage: "",
      showOptions: true,
    };
  },
  methods: {
    handleUserResponse(response) {
      this.chatMessages.push({
        id: this.chatMessages.length + 1,
        text: response,
        type: "user",
      });
      this.showOptions = false;
      this.handleBotResponse(response);
    },
    getUserInput(event) {
      if (event.key === "Enter") {
        const userInput = event.target.value;
        this.handleUserResponse(userInput);
        event.target.value = "";
      }
    },
    handleBotResponse(userInput) {
      let botReply = "";
      if (userInput === "Заказать пиццу 🍕") {
        botReply = "Хорошо, я закажу пиццу. Что еще могу сделать?";
      } else if (userInput === "Установить будильник ⏰") {
        botReply = "Будильник установлен. Чем еще могу помочь?";
      } else if (userInput === "Вывести погоду ⛅") {
        botReply = "Погода сегодня отличная! Что дальше?";
      } else {
        botReply = "Извините, я не понимаю. Попробуйте выбрать другой вариант.";
        this.showOptions = true;
      }

      this.chatMessages.push({
        id: this.chatMessages.length + 1,
        text: botReply,
        type: "bot",
      });

      if (
        botReply !==
        "Извините, я не понимаю. Попробуйте выбрать другой вариант."
      ) {
        this.showOptions = true;
      }
    },
  },
};
</script>
