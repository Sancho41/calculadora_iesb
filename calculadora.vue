<template>
  <div class="container">
    <header>
      <img src="https://www.infoenem.com.br/wp-content/uploads/2016/12/iesb.jpg" alt />
      <h1>Calculadora de média</h1>
    </header>

    <article>
      <section>
        <h2>Digite aqui as suas notas:</h2>

        <div class="input-group">
          <label for="nota-p1">P1:</label>
          <input v-model="p1" type="number" step="any" id="nota-p1" />
        </div>

        <div class="input-group">
          <label for="nota-p2">P2:</label>
          <input v-model="p2" type="number" step="any" id="nota-p2" />
        </div>
      </section>

      <section>
        <h2>Aqui estão seus resultados:</h2>

        <table>
          <tr v-if="p1 != 0 && p2 == 0">
            <td>Nota necessária para a P2:</td>
            <td>{{ calcP2 }}</td>
          </tr>
          <tr v-if="p2 != 0 && !aproved">
            <td>Nota necessária para a P3:</td>
            <td>{{ calcP3 }}</td>
          </tr>
          <tr v-if="p2 != 0 && !aproved">
            <td>Prova a ser substituida pela P3:</td>
            <td>{{ subs }}</td>
          </tr>
          <tr>
            <td>Média atual:</td>
            <td>{{ res }}</td>
          </tr>
          <tr v-if="p2 != 0">
            <td>Situação atual:</td>
            <td v-if="aproved">Aprovado :)</td>
            <td v-else>Reprovado :(</td>
          </tr>
        </table>
      </section>
    </article>
    <small v-if="p2 > 10 || p1 > 10">É um prodígio tirando mais que a média...</small>
    <small v-if="p1 != 0 && p2 == 0">
      Não é possível fazer a
      <strong>P3</strong> se não tirar mais que
      <strong>0</strong> na
      <strong>P2.</strong>
    </small>
  </div>
</template>

<script>
export default {
  data() {
    return {
      p1: 0,
      p2: 0
    };
  },

  computed: {
    calcP2() {
      let nota = 0;

      nota = (this.p1 * 0.4 * -1 + 5) / 0.6;

      if (nota >= 0) return parseFloat(nota.toFixed(2));
      return 0;
    },
    calcP3() {
      let nota1, nota2, nota;

      nota1 = (this.p1 * 0.4 * -1 + 5) / 0.6;
      nota2 = (this.p2 * 0.6 * -1 + 5) / 0.4;

      nota =
        nota1 > nota2
          ? parseFloat(nota1.toFixed(2))
          : parseFloat(nota2.toFixed(2));
      if (nota1 > 5 || nota2 > 5)
        nota =
          nota1 < nota2
            ? parseFloat(nota1.toFixed(2))
            : parseFloat(nota2.toFixed(2));

      return nota >= 0 ? nota : 0;
    },
    subs() {
      let nota1, nota2;

      nota1 = (this.p1 * 0.4 * -1 + 5) / 0.6;
      nota2 = (this.p2 * 0.6 * -1 + 5) / 0.4;

      if (nota1 > 5 || nota2 > 5) return nota1 < nota2 ? "P2" : "P1";
      return nota1 > nota2 ? "P2" : "P1";
    },
    res() {
      let nota = this.p1 * 0.4 + this.p2 * 0.6;
      return parseFloat(nota.toFixed(2));
    },
    aproved() {
      return this.res >= 5;
    }
  }
};
</script>

<style>
*,
*::before,
*::after {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background: #fff;
  color: white;
  min-height: 100vh;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.container {
  display: block;
  padding: 20px;
  background: #ec0000;
  border-radius: 3px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
}

header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 10px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.192);
}

header h1 {
  margin: 10px;
}

header img {
  width: 100px;
}

article {
  display: flex;
}

section {
  padding: 10px;
  margin: 10px;
  border-radius: 2px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
  background: #2b2d42;
  display: flex;
  flex-direction: column;
}

table {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

tr {
  display: flex;
  justify-content: space-between;
  padding-bottom: 10px;
  margin-bottom: 10px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.192);
}

td:last-child {
  font-weight: bolder;
  font-size: 1.2rem;
}

section h2 {
  margin-bottom: 10px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.192);
}

.input-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

input {
  border-radius: 2px;
  border: none;
  padding: 5px;
  font-size: 1rem;
  font-weight: bold;
  width: 75px;
  text-align: center;
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
  }

  .container {
    padding: 5px;
  }

  article {
    flex-direction: column;
  }

  section {
    margin: 10px 0;
  }
}
</style>