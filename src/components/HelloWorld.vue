<template>
  <div class="hello">
    <h1>{{ x }} {{ op }} {{ y }}</h1>
    <input type="number" name="answer" placeholder="Ergebnis" @keyup.enter="checkAnswer" v-model="answer"/>
    <p>{{ msg }}</p>
    <p>Richtig: {{ correctAnswers }} - Falsch: {{ wrongAnswers }}</p>
    <section>
      <button @click="checkAnswer">Überprüfen</button><button @click="reset">Zurücksetzen</button>
    </section>
    <section>
      <ul>
        <li v-for="l in log" :key="l">
           {{ l }}
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
function rnd(min, max) {
  return Math.floor(Math.random()*(max-min+1)+min);
}

function initQuestion() {
  let op = rnd(1,4);

  var opStr = '';
  var result = 0;
  var x, y;

  switch (op) {
    case 1: // Add
      opStr = '+';
      x = rnd(1,100);
      y = rnd(1,100-x);
      result = x+y;
      break;
    case 2: // Sub
      opStr = '-';
      x = rnd(1,100);
      y = rnd(1,x);
      result = x-y;
      break;
    case 3: // Mul
      opStr = 'x';
      x = rnd(1,10);
      y = rnd(1,10);
      result = x*y;
      break;
    case 4: // Div
      opStr = '/';
      y = rnd(1,10);
      x = y*rnd(1,10);
      result = x/y;
      break;
  }

  return { x:x, y:y, op:opStr, result:result};
}

export default {
  name: 'HelloWorld',
  data () {
    return { 
     op: 'x',
     x: 1,
     y: 1,
     correctAnswers: 0,
     wrongAnswers: 0,
     msg: '',
     answer: 0,
     question: null,
     log: []
    }
  },
  methods: {
    init () {
      var q = initQuestion();
      this.x = q.x;
      this.y = q.y;
      this.op = q.op;
      //this.msg = q.result;
      this.answer = '';
      this.question = q;
    },
    appendLog(q,a) {
      var l = `${q.x}${q.op}${q.y}. Deine Antwort: ${a}. Richtige Antwort: ${q.result}`;
      this.log.push(l);
    },
    checkAnswer() {
      if (this.answer == this.question.result) {
        this.correctAnswers += 1;
        this.msg = 'Richtig!';
      } else {
        this.wrongAnswers += 1;
        this.msg = 'Falsch!';
      }
      this.appendLog(this.question, this.answer);
      this.init();
    },
    reset() {
      this.correctAnswers = 0;
      this.wrongAnswers = 0;
      this.log = [];
      this.msg = '';
      this.init();
    }
  },
  mounted() {
    this.init();
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  font-size: 10px;
}
a {
  color: #42b983;
}
</style>
