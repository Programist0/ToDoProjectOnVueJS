<template>
  <div id="app">
    <div class="card">
      <h1 :style="{
				color: inputValue.length < 10 ? 'darkred' : 'darkblue',
				fontSize: inputValue.length > 10 ? '29px' : '28px'
			}">{{ title }}</h1>
      <div class="form-control">
        <input type="time" v-model="time">
        <div class="special_info">{{info}}</div>
        <input type="text" :placeholder="placeholderString"
               :value="inputValue"
               @input="inputChangeHandler"
               @keypress.enter="addNewNote"
        />
        <div class="important"><input type="checkbox" v-model="important">Important note</div>
        <div class="important"><input type="checkbox" v-model="first">Take the note top of the list</div>
      </div>
      <button class="btn" @click="addNewNote">Add</button>
      <span :class="{
				'primary': true,
				'bold': inputValue.length > 5
			}"></span>
      <span :class="['bold', {'primary': inputValue.length > 5}]"></span>
      <hr/>
      <ul :class="{'list': true, 'list-column': column}" v-if="notes.length !== 0">
        <li class="list-counter">
          View:
          <input type="checkbox" id="my_checkbox" v-model="column" name="column">
          <label for="my_checkbox" :class="['label_checkbox', {'hide': column}]">blocks</label>
          <label for="my_checkbox" :class="['label_checkbox', {'hide': !column}]">rows</label>
        </li>
        <li :class="{'list-item': true, 'list-item-column': column}"
            v-for="(note, index) in notes"
            :key="note"
            :style="{backgroundColor: importantNotes[index] === true < 10 ? '#ffc6c6' : '#fff6c6', fontWeight: importantNotes[index] === true < 10 ? '800' : '300'}"
        >{{note}}<strong>{{timeOfNote[index]}}</strong><button class="btn danger" @click="deleteNote(index, $event)">Delete note</button></li>
        <li class="list-counter">
          <strong>Count of notes: {{notes.length}}</strong>
        </li>
      </ul>
      <div v-else>No notes here, add new one!</div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {

  },
  data: function () {
    return {
      counter: 1,
      title: 'What to do today?',
      placeholderString: 'Add your first note!',
      inputValue: '',
      notes: [],
      timeOfNote: [],
      info: '',
      important: false,
      importantNotes: [],
      time: '12:00',
      first: false,
      column: false
    }
  },
  methods:{
    inputChangeHandler(event){
      this.inputValue = event.target.value;
    },
    addNewNote(){
      if(this.inputValue !== ''){
        if(this.first){
          this.notes.unshift(this.inputValue);
          this.importantNotes.unshift(this.important);
          this.timeOfNote.unshift(this.time);
        } else{
          this.notes.push(this.inputValue);
          this.importantNotes.push(this.important);
          this.timeOfNote.push(this.time);
        }
        this.time = '12:00';
        this.important = false;
        this.first = false;
        this.inputValue = '';
      }
      if(this.counter >= 1){
        this.placeholderString = 'Add your new note here..';
      }
    },
    deleteNote(index){
      this.notes.splice(index, 1);
      this.importantNotes.splice(index, 1);
      this.timeOfNote.splice(index, 1);
    }
  },
  watch:{
    inputValue(value){
      if(value.length < 50) {
        this.info = '';
      }
      if(value.length > 50){
        this.info = 'Max length of note!';
        this.inputValue = value.substring(0, value.length - 1);
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap');

* {
  box-sizing: border-box;
}

body {
  font-family: Inter, Roboto, Oxygen, Fira Sans, Helvetica Neue, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 16px;
  color: #2c3e50;
  background: #2c3e50;
}

hr {
  margin: 1rem 0;
}

strong,
.bold {
  font-weight: 500;
}

ol,
p,
ul {
  line-height: 1.7;
}

a {
  color: #3eaf7c;
  font-weight: 500;
  text-decoration: none;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 500;
  line-height: 1.45;
}

h1 {
  font-size: 2.2rem;
  font-weight: 600;
}

h2 {
  font-size: 1.65rem;
  padding-bottom: 0.3rem;
  border-bottom: 1px solid #eaecef;
}

h3 {
  font-size: 1.35rem;
}

/*.primary {*/
/*    color: #42b983;*/
/*}*/

/*.danger {*/
/*    color: #e53935;*/
/*}*/

.btn {
  color: #42b983;
  position: relative;
  place-content: center;
  place-items: center;
  width: fit-content;
  border-radius: 99px;
  letter-spacing: 0.05em;
  border: 1px solid #42b983;
  text-decoration: none;
  text-transform: uppercase;
  margin-right: 10px;
  padding: 0.5rem 1.5rem;
  white-space: nowrap;
  font-weight: 700;
  outline: none;
  background: #fff;
  transition: all 0.22s;
}

.btn:hover {
  cursor: pointer;
  opacity: 0.8;
}

.btn:disabled {
  cursor: not-allowed;
  opacity: 1!important;
  background: #eee!important;
  border-color: #ddd!important;
  color: #999!important;
}

.btn:active {
  box-shadow: inset 1px 1px 1px rgba(0, 0, 0, 0.3);
}

.btn.danger {
  background: #e53935;
  color: #fff;
  border-color: #e53935;
}

.form-control {
  position: relative;
  margin-bottom: 0.5rem;
}

.form-control input,
.form-control select {
  margin: 0;
  outline: none;
  border: 2px solid #ccc;
  display: block;
  width: 100%;
  color: #2c3e50;
  padding: 0.5rem 1.5rem;
  border-radius: 3px;
  font-size: 1rem;
}

.form-control small {
  color: #e53935;
}

.form-control.invalid input {
  border-color: #e53935;
}

.form-checkbox .checkbox input {
  margin-right: 1rem;
}
.hide{
  display: none;
}
.form-control label {
  display: block;
  margin: 0 0 0.3rem 0.3rem;
  font-weight: 500;
}

.form-control input:active,
.form-control input:focus {
  transition: border 0.22s;
  border: 2px solid #42b983;
}

.card {
  overflow: hidden;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 10px;
  box-shadow: 2px 3px 10px rgba(0, 0, 0, 0.2);
  background: #fff;
}

.list {
  margin: 0;
  padding: 0;
  list-style: none;
}

.list-item {
  border: 1px solid #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0;
  transition: .22s all;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
  padding-left: 10px;
}

.list-item:hover {
  background: #eee;
}

.alert p {
  color: #222;
  line-height: 1.7;
}


@-webkit-keyframes load3 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes load3 {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

.navbar-menu li {
  margin-right: 2rem;
}

.navbar-menu li a {
  color: #2c3e50;
  text-decoration: none;
}

.list-item-column{
  flex-direction: column;
  width: 50%;
}

.list-column{
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.list-counter{
  width: 100%;
  text-align: right;
}
#my_checkbox[type="checkbox"]{
  position: absolute;
  z-index: -1;
  opacity: 0;
}
.label_checkbox{
  cursor: pointer;
  color: #111;
  font-weight: bold;
}
.important{
  display: flex;
  align-items: center;
  margin-top: 10px;
}
.important input{
  width: 3%;
}
.special_info{
  min-height: 10px;
  color: #f00;
}
</style>
