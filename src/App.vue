<template>
  <div id="app">
    <h1>Jogo da Forca</h1>

    <section v-if="tela === 'inicio'" id="inicio">
      <!-- Fórmulário será dinâmico -->
      <Formulario
        v-if="etapa === 'palavra'"
        title="Defina a Palavra"
        button="Próximo"
        :action="setPalavra"
      />
      <!-- Aqui será a condicional para definição da dica -->
      <Formulario
        v-if="etapa === 'dica'"
        title="Defina a dica"
        button="Definir"
        :action="setDica"
      />
    </section>

    <section v-if="tela === 'jogo'" id="jogo">
      <Jogo
        :erros="erros"
        :palavra="palavra"
        :dica="dica"
        :verificarLetra="verificarLetra"
        :etapa="etapa"
        :letras="letras"
        :jogar="jogar"
        :jogarNovamente="jogarNovamente"
      />
    </section>
  </div>
</template>

<script>
import "./css/global.css";
import Formulario from "./components/Formulario";
import Jogo from "./components/Jogo";

export default {
  name: "App",
  data() {
    return {
      tela: "inicio",
      //Etapa será passado para Jogo que passará para Palavra que passará para Final 
      etapa: "palavra",
      palavra: "",
      dica: "",
      erros: 0,
      letras: [],
    };
  },
  components: {
    Formulario,
    Jogo,
  },
  methods: {
    setPalavra: function (palavra) {
      this.palavra = palavra;
      this.etapa = "dica";
    },
    setDica: function (dica) {
      this.dica = dica;
      this.etapa = "jogo";
      this.tela = "jogo";
    },
    verificarLetra: function (letra) {
      return this.letras.find(
        (item) => item.toLowerCase() === letra.toLowerCase()
      );
    },
    jogar: function (letra) {
      //adiciona letra jogada
      this.letras.push(letra);

      //validar erro
      this.verificarErros(letra);
    },

    verificarErros: function (letra) {
      //acerto
      if (this.palavra.toLowerCase().indexOf(letra.toLowerCase()) >= 0) {
        return this.verificarAcertos();
      }
      //erros
      this.erros++;
      //enforcado
      if (this.erros == 6) {
        this.etapa = "enforcado";
      }
    },
    verificarAcertos: function () {
      let letrasUnicas = [...new Set(this.palavra.split(""))];
      if (letrasUnicas.length === this.letras.length - this.erros) {
        this.etapa = "ganhador";
      }
    },
    jogarNovamente: function () {
      this.palavra = "";
      this.dica = "";
      this.erros = 0;
      this.letras = [];
      this.tela = "inicio";
      this.etapa = "palavra";
    },
  },
};
</script>

<style>
#app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
