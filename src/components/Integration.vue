<template>
  <div>
    <span>Memed container</span>
    <button id="botaoShowPrescricao" style="margin-left: 10px">
      Gerar prescricao
    </button>
    <div class="memed-container" id="memed-container"></div>
  </div>
</template>

<script>
export default {
  name: "Integration",
  props: {},
  data: () => {
    return {
      body: document.querySelector("body"),
      token:'' // INSIRA TOKEN DO MÉDICO
    };
  },
  methods: {
    initMemed: function () {
      let script = document.createElement("script");
      script.setAttribute("type", "text/javascript");
      script.setAttribute("data-color", "#1abc9c");
      script.setAttribute(
        "data-token",
        this.token
      );
      script.setAttribute("data-container", "memed-container");
      script.src =
        "https://sandbox.memed.com.br/modulos/plataforma.sinapse-prescricao/build/sinapse-prescricao.min.js";
      script.onload = function (data) {
        data.path[3].defaultView.MdSinapsePrescricao.event.add(
          "core:moduleInit",
          function moduleInitHandler(module) {
            console.log(module);
            if (module.name === "plataforma.prescricao") {
              document
                .getElementById("botaoShowPrescricao")
                .addEventListener("click", function () {
                  data.path[3].defaultView.MdHub.command
                    .send("plataforma.prescricao", "setPaciente", {
                      nome: "José da Silva",
                      endereco: "Rua da Saúde, 123",
                      cidade: "São Paulo",
                      telefone: "11999999999",
                      peso: 75,
                      altura: 1.8,
                      nome_mae: "Nome da mãe",
                      dificuldade_locomocao: true,
                      idExterno: 111,
                    })
                    .then(function () {
                      data.path[3].defaultView.MdHub.module.show("plataforma.prescricao");
                    });
                });
            }
          }
        );
      };
      this.body.appendChild(script);
    },
  },
  mounted() {
    this.initMemed();
  },
};
</script>

<style scoped>
.memed-container {
  margin: 0 auto;
  border: 2px solid black;
  /* Largura mínima 820px */
  width: 820px;
  height: 710px; 
}
</style>