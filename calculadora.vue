<template>
  <div class="container">
    <header>
      <h1>Calculadora de média</h1>
    </header>

    <article>
      <section>
        <input v-model="p1" placeholder="Prova 1" type="number" step="any" min="0" max="10" />
        <input v-model="p2" placeholder="Prova 2" type="number" step="any" min="0" max="10" />
      </section>

      <section>
        <div class="row" :class="p1 && !p2 ? 'active' : ''">
          <p>Necessário para a P2</p>
          <p class="value">{{ calcP2 | round }}</p>
        </div>
        <div class="row" :class="p1 && p2 && !approved ? 'active' : ''">
          <p>Necessário para a P3</p>
          <p class="value">{{ calcP3 | round }}</p>
        </div>
        <div class="row" :class="p1 && p2 && !approved ? 'active' : ''">
          <p>Prova a ser substituída</p>
          <p class="value">{{ p1 && p2 ? subs : '-' }}</p>
        </div>
        <div class="row" :class="p1 && p2 ? 'active' : ''">
          <p>Média</p>
          <p class="value">{{ media | round }}</p>
        </div>
        <div class="row" :class="p1 && p2 ? 'active' : ''">
          <p>Situação atual</p>
          <p class="value">{{ p1 && p2 ? ( approved ? 'Aprovado' : 'Reprovado' ) : '-' }}</p>
        </div>
      </section>
    </article>
  </div>
</template>

<script>
export default {
  data() {
    return {
      p1: "",
      p2: "",
      media_ap: 4.96
    };
  },

  filters: {
    round: function(number) {
      return number.toFixed(2);
    }
  },

  computed: {
    calcP2() {
      let nota = 0;

      nota = (this.p1 * 0.4 * -1 + 5) / 0.6;

      if (nota >= 0) return nota;
      return 0;
    },
    calcP3() {
      let nota1, nota2, nota;

      nota1 = (this.p1 * 0.4 * -1 + 5) / 0.6;
      nota2 = (this.p2 * 0.6 * -1 + 5) / 0.4;

      nota = nota1 > nota2 ? nota1 : nota2;
      if (nota1 > 5 || nota2 > 5) nota = nota1 < nota2 ? nota1 : nota2;

      return nota >= 0 ? nota : 0;
    },
    subs() {
      let nota1, nota2;

      nota1 = (5 - this.p1 * 0.4) / 0.6;
      nota2 = (5 - this.p2 * 0.6) / 0.4;

      if (nota1 > this.media_ap || nota2 > this.media_ap)
        return nota1 < nota2 ? "P2" : "P1";
      return nota1 > nota2 ? "P2" : "P1";
    },
    media() {
      let nota = this.p1 * 0.4 + this.p2 * 0.6;
      return nota;
    },
    approved() {
      return this.media >= this.media_ap;
    }
  }
};
</script>

<style>
.container {
  display: block;
  padding: 30px;
  border-radius: 15px;
  border: 2px solid #313131;
  transition: border 1s ease-in-out;
}

.container:hover {
  animation: glowRed 5s infinite alternate;
  border: 2px solid #ec0000;
}

@keyframes glowRed {
  from {
    box-shadow: 0 0 0px #ec0000, inset 0 0 0px #ec0000;
  }
  to {
    box-shadow: 0 0 15px #ec0000, inset 0 0 15px #ec0000;
  }
}

header {
  text-align: center;
  color: #999999;
}

article {
  display: flex;
}

section {
  padding: 30px;
  margin: 30px;
  border-radius: 15px;
  box-shadow: -5px -5px 10px rgba(255, 255, 255, 0.1),
    5px 5px 10px rgba(0, 0, 0, 0.3),
    inset -5px -5px 10px rgba(255, 255, 255, 0.1),
    inset 5px 5px 10px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  width: 300px;
  height: 250px;
  justify-content: space-around;
  align-items: space-between;
  border: 2px solid #581a1a;
}

.active {
  color: white !important;
  text-shadow: 0 0 2px white;
}

.row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #414141;
  transition: color 300ms ease-in-out;
}

.row .value {
  font-size: 1.5em;
}

input {
  border-radius: 15px;
  border: none;
  width: 100%;
  padding: 10px 20px;
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
  margin: 10px 0;

  border: 3px solid #313131;
  background: none;
  color: white;
  transition: border 300ms ease-in-out;
}

input:focus,
input:active {
  border: 3px solid white;
  outline: none;
  animation: glowWhite 5s infinite alternate;
}

@keyframes glowWhite {
  from {
    box-shadow: 0 0 0 white, inset 0 0 0 white;
  }
  to {
    box-shadow: 0 0 5px white, inset 0 0 5px white;
  }
}

input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  margin: 0;
}

@media (max-width: 768px) {
  header h1 {
    font-size: 1.5rem;
    margin: 20px 0;
  }

  .container,
  .container:hover {
    padding: 5px;
    border: none;
    animation: none;
  }

  article {
    flex-direction: column;
  }

  section {
    margin: 10px;
  }
}
</style>