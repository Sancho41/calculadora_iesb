<template>
    <div class="container">
        
        <header>
            <img src="https://www.infoenem.com.br/wp-content/uploads/2016/12/iesb.jpg" alt="">
            <h1>Calculadora de média</h1>
        </header>


        <article>
            <section>
                <h2>Digite aqui as suas notas:</h2>

                <div class="input-group">
                    <label for="nota-p1">P1:</label>
                    <input v-model="p1" type="number" step="any" id="nota-p1">
                </div>

                <div class="input-group">
                    <label for="nota-p2">P2:</label>
                    <input v-model="p2" type="number" step="any" id="nota-p2">
                </div>


                <div v-if="p1 != 0" class="input-group">
                    <label for="nota-has-edad">Tem edad:</label>
                    <div class="toggle">
                        <input v-model="hasEdad" type="checkbox" id="nota-has-edad">
                        <label for="nota-has-edad"></label>
                    </div>
                </div>

                <div class="input-group" v-if="hasEdad">
                    <label for="nota-edad">EDAD:</label>
                    <input v-model="edad" type="number" step="any" id="nota-edad">
                </div>
            </section>

            <section>
                <h2>Aqui estão seus resultados: </h2>

                <table>
                    <tr v-if="p1 != 0 && p2 == 0">
                        <td>Nota necessária para a P2: </td>
                        <td>{{ calcP2 }}</td>
                    </tr>
                    <tr v-if="p2 != 0 && !aproved">
                        <td>Nota necessária para a P3:</td>
                        <td>{{ calcP3 }}</td>
                    </tr>
                    <tr v-if="p2 != 0 && !aproved">
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
        <small v-if="p2 > 10 || p1 > 10">É um prodígio tirando mais que a média...</small>
        <small v-if="p1 != 0 && p2 == 0">Não é possível fazer a <strong>P3</strong> se não tirar mais que <strong>0</strong> na <strong>P2.</strong></small>
    </div>
</template>

<script>
export default {
    data () {
        return {
            p1: 0,
            p2: 0,
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
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background: #262626;
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }

    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        background: #ea0001;
        border-radius: 5px;
        box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
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
        margin-top: 20px;
    }

    header img {
        width: 100px;
        margin: 0 auto;
    }

    article {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
    }

    section {
        border: 1px white;
        border-style: dotted;
        border-radius: 5px;
        display: flex;
        flex-direction: column;
        padding: 20px;
        min-width: 280px;
        max-width: 500px;
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

    .input-group input[type="number"]{
        height: 25px;
        border-radius: 5px;
        font-weight: bold;
        width: 50px;
        text-align: center;
        font-size: 1.3rem;
    }

    input[type=number]::-webkit-inner-spin-button, 
    input[type=number]::-webkit-outer-spin-button { 
        -webkit-appearance: none;
        -moz-appearance: none;
        appearance: none;
        margin: 0; 
    }

    .toggle input {
        display: none;
    }

    .toggle label{
        width: 48px;
        height: 26px;
        background: #8D909B;
        position: relative;
        display: block;
        border-radius: 16px;
        cursor: pointer;
        transition: all ease-in-out 300ms;
    }

    .toggle label::before {
        content:'';
        background-color: #EEF0F2;
        position: absolute;
        display: block;
        margin: 2px;
        width: 22px;
        height: 22px;
        border-radius: 50%;
        transition: all ease-in-out 300ms;
        right: 100%;
        transform: translateX(calc(100% + 4px))
    }

    .toggle input:checked ~ label{
        background: #08A045;
    }

    .toggle input:checked ~ label::before{
        right: 0;
        transform: translateX(0)
    }

    table {
        display: flex;
        flex-direction: column;
        height: calc(100% - 20px);
        justify-content: space-around;
    }

    tr {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin: 5px;
    }

    td:last-child {
        text-align: right;
        font-size: 1.2rem;
        font-weight: bold;
    }

    small {
        margin-bottom: 20px;
    }

</style>