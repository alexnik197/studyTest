<template>
  <header>
    <span>Online Education</span>
    <div class="theme">
      <span @click="changeTheme" v-if="isDarkTheme" class="theme-icon">🌙</span>
      <span @click="changeTheme" v-else class="theme-icon">☀️</span>
    </div>
  </header>
  <main>
    <aside>
      <h3>Создание</h3>
      <form @submit.prevent="sendData">

        <select class="input-task" v-model="selectedBlockTitle">
          <option disabled selected="selected">Выберите тип</option>
          <option v-for=" block in blockType" :value="block.value" :key="block.value">{{ block.text }}</option>
        </select>

        <h3 class="block-type__text">{{ blockTypeText }}</h3>

        <div v-if="selectedBlockTitle == 0" class="study-subj">
          <select class="input-task" v-model="selectedObj">
            <option v-for="object in studyObj" :value="object.value === 0 ?
              0 : object.text" :key="object.value">{{ object.text }}</option>
          </select>
        </div>

        <div v-if="selectedBlockTitle == 1" class="study-subj">
          <textarea v-model="taskDescription" placeholder="Добавьте описание" />
        </div>

        <div v-if="selectedBlockTitle == 2" class="study-subj">
          <textarea v-model="oneChoice[0].description" placeholder="Описание задачи" contenteditable />
          <div class="choice-list">

            <div v-for="(choice, idx) in oneChoice.slice(1)" class="choice-task" :key="idx">
              <input type="radio" name="check-test--one" :id="idx" :value="choice.value"
                v-model="oneChoice[0].rightChoice" />
              <input type="text" v-model="choice.description" />
            </div>
            <button @click.prevent.stop=addOneChoice()>+</button>
          </div>
        </div>

        <div v-if="selectedBlockTitle == 3" class="study-subj">
          <textarea v-model="multiChoice.description" placeholder="Описание задачи" contenteditable />
          <div class="choice-list">
            <div class="choice-task">
              <input type="checkbox" name="check-test--multi" value="1">
              <input class="choice-text" type="text">
            </div>
            <div class="choice-task">
              <input type="checkbox" name="check-test--multi" value="1">
              <input class="choice-text" type="text">
            </div>
          </div>
        </div>
      </form>

      <input v-if="selectedBlockTitle === 0 && selectedObj === 0" v-model="myStudyObj" type="text">
      <button @click="sendData">Создать</button>
    </aside>

    <article>
      <ul>
        <test-section :taskList="taskList" />
      </ul>
    </article>
  </main>
</template>

<script>
import TestSection from "./components/TestSection.vue";

export default {
  components: { TestSection },

  data() {
    return {
      taskList: [],

      selectedBlockTitle: null,
      selectedObj: null,
      taskDescription: "",

      studyObj: [
        { text: "Свой вариант", value: 0 },
        { text: "Математика", value: 1 },
        { text: "Русский язык", value: 2 },
        { text: "Английский язык", value: 3 },
      ],

      blockType: [
        { text: "Заголовок", value: 0 },
        { text: "Описание", value: 1 },
        { text: "Задача с выбором одного варианта", value: 2 },
        { text: "Задача с выбором вариантов", value: 3 },
      ],

      oneChoice: [{ description: "", rightChoice: null }, { value: 0, description: '' }, { value: 1, description: '' }],

      multiChoice: {
        description: "",
        rightChoice: [],
        variant1: "",
        variant2: ""
      },

      myStudyObj: "",
      isDarkTheme: false,
    };
  },

  computed: {
    blockTypeText() {
      if (this.selectedBlockTitle) {
        const selectedType = this.blockType.filter((type) => type.value == this.selectedBlockTitle);
        return selectedType[0].text;
      }
    },
    selectedTitle() {
      if (this.selectedObj === 0) return this.myStudyObj;
      else return this.selectedObj;
    }
  },

  methods: {
    sendData() {
      if (this.selectedBlockTitle === 0) {
        this.taskList.push({ title: this.selectedBlockTitle, info: this.selectedTitle });
      }
      else if (this.selectedBlockTitle === 1) {
        this.taskList.push({ title: this.selectedBlockTitle, info: this.taskDescription });
      }

      else if (this.selectedBlockTitle === 2) {
        console.log(this.oneChoice);
        this.taskList.push({ title: this.selectedBlockTitle, info: this.oneChoice });
      }

    },
    addOneChoice() {
      const choicesNumber = this.oneChoice.length;
      this.oneChoice.push({ value: choicesNumber, description: "" });
    },
    changeTheme() {
      this.isDarkTheme = !this.isDarkTheme;
    }
  }

};
</script>

<style>
body {
  display: flex;
  justify-content: center;
  font-family: sans-serif;
  background-color: #282828;
  color: #d3d3d3;
  margin: 0;
}

main {
  display: flex;
  flex-direction: row;
  gap: 1rem;
  padding: 15px 0px;
  border-radius: 10px;
  width: 60rem;
  margin: auto;
}

article {
  width: 700px;
  min-height: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #d3d3d3;
  color: #282828;
  padding: 1rem 3rem;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
}

aside {
  width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;

  background-color: #d3d3d3;
  color: #282828;

  padding: 1rem 1.5rem;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  gap: 1rem;
}

textarea {
  width: 100%;
  min-height: 2rem;
}

h1,
h2,
h3 {
  margin: 0;
}

header {
  background-color: #d3d3d3;
  color: #282828;
  display: flex;
  align-items: center;
  justify-content: space-around;
  width: 100vw;
  height: 4rem;
  margin-bottom: 1rem;

  font-size: 2rem;
  line-height: 1.5;
}

input {
  height: 1.5rem;
  width: max-content;
  outline: none;
  border-top-left-radius: 5px;
  border: none;
  border: 1px solid #282828;
  font-size: 1rem;
  padding: 5px;
}

input[type=checkbox] {
  cursor: pointer;
  width: 1.5rem;
  height: 1.5rem;
}

input[type=radio] {
  cursor: pointer;
  width: 1.5rem;
  height: 1.5rem;
}

ul {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1rem;
  padding: 0;
  list-style-type: none;
}

p {
  margin: 0;
}

button {
  background-color: #48d87f;
  color: #282828;
  font-weight: 700;
  border-radius: 10px;
  border: 2px solid #282828;
  height: max-content;
  font-size: 1rem;
  padding: 5px 10px;
  cursor: pointer;
}

.block-type__text {
  text-align: center;
}

.theme-icon {
  cursor: pointer;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.study-subj {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
}

.input-task {
  width: 100%;
}

.choice-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.choice-task {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.choice-text {
  border-radius: 5px;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
