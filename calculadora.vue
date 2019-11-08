<template>
    <div class="container">
        
        <header>
            <img src="https://www.infoenem.com.br/wp-content/uploads/2016/12/iesb.jpg" alt="">
            <h1>Calculadora</h1>
        </header>


        <article>
            <section>
                <h2>Digite aqui as suas notas:</h2>

                <div class="input-group">
                    <label for="nota-p1">P1:</label>
                    <input v-model="p1" type="number" step="any" name="nota-p1">
                </div>

                <div class="input-group">
                    <label for="nota-p2">P2:</label>
                    <input v-model="p2" type="number" step="any" name="nota-p2">
                </div>


                <div class="input-group">
                    <label for="nota-has-edad">Tem edad?:</label>
                    <input v-model="hasEdad" type="checkbox" name="nota-has-edad">
                </div>

                <div class="input-group" v-if="hasEdad">
                    <label for="nota-edad">EDAD:</label>
                    <input v-model="edad" type="number" step="any" name="nota-edad">
                </div>
            </section>

            <section>
                <h2>Aqui estão seus resultados: </h2>

                <table>
                    <tr v-if="p1 != 0 && p2 == 0">
                        <td>Nota necessária para a P2: </td>
                        <td>{{ calcP2 }}</td>
                    </tr>
                    <tr v-if="p2 != 0">
                        <td>Nota necessária para a P3:</td>
                        <td>{{ calcP3 }}</td>
                    </tr>
                    <tr v-if="p2 != 0">
                        <td>Prova a ser substituida pela P3:</td>
                        <td>P{{ subs }}</td>
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

    </div>
</template>

<script>
export default {
    data () {
        return {
            p1: 3,
            p2: 3,
            edad: 0,
            hasEdad: false,
        }
    },

    computed: {
        calcP2 () {
            let nota = 0
            
            nota = ((this.p1 * .4 * -1) + 5) / .6
            if (this.hasEdad)
                nota = ((this.p1 * .4 * -1) + 5 + (this.edad * .03 * -1)) / .57
            
            if (nota >= 0)
                return parseFloat(nota.toFixed(2));
            return 0
        },
        calcP3 () {
            let nota1, nota2, nota;
            
            nota1 = ((this.p1 * .4 * -1) + 5) / .6
            if (this.hasEdad)
                nota1 = ((this.p1 * .4 * -1) + 5 + (this.edad * .03 * -1)) / .57
            
            
            nota2 = ((this.p2 * .6 * -1) + 5) / .4
            if (this.hasEdad)
                nota2 = ((this.p2 * .57 * -1) + 5 + (this.edad * .03 * -1)) / .4
            
            nota = nota1 > nota2 ? parseFloat(nota1.toFixed(2)) : parseFloat(nota2.toFixed(2))
            if (nota1 > 5 || nota2 > 5)
                nota = nota1 < nota2 ? parseFloat(nota1.toFixed(2)) : parseFloat(nota2.toFixed(2))
            
            return nota >= 0 ? nota : 0;
        },
        subs () {
            let nota1, nota2;
            
            nota1 = ((this.p1 * .4 * -1) + 5) / .6
            if (this.hasEdad)
                nota1 = ((this.p1 * .4 * -1) + 5 + (this.edad * .03 * -1)) / .57
            
            
            nota2 = ((this.p2 * .6 * -1) + 5) / .4
            if (this.hasEdad)
                nota2 = ((this.p2 * .57 * -1) + 5 + (this.edad * .03 * -1)) / .4
            
            
            if (nota1 > 5 || nota2 > 5)
                return nota1 < nota2 ? "2" : "1"
            return nota1 > nota2 ? "2" : "1"
        },
        res () {
            let nota = (this.p1 * .4) + (this.p2 * .6);
            if (this.hasEdad)
                nota = (this.p1 * .4) + (this.p2 * .57) + (this.edad * .03);
            
            return parseFloat(nota.toFixed(2));
        },
        aproved () {
            return this.res >= 5
        }
    }
}
</script>

<style>
    *, *::before, *::after {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'Courier New', Courier, monospace;
        background: #e44339;
        color: white;
    }

    .container {
        width: 100vw;
        height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    header {
        position: relative;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 25px;
    }

    header h1 {
        text-transform: uppercase;
        text-align: center;
    }

    header img {
        width: 100px;
        margin: 0 auto;
        margin: 25px;
    }

    article {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
    }

    section {
        border: 1px solid white;
        display: flex;
        flex-direction: column;
        padding: 20px;
        width: 400px;
        margin: 20px;
    }

    section .input-group {
        margin: 10px;
    }

    .input-group {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }


</style>