<template>
  Açaízinho?
  Participantes
  <v-btn @click="addParticipante">
    Add
  </v-btn>

  <v-text-field
    label="Total combo"
    v-model.number="totalCombo"
    variant="outlined"
  ></v-text-field>

  <div
    v-for="participante in participantes"
    :key="participante.id"
  >
    <p>{{ participante.nome }} paga: {{ (totalParaCada + adicionais.find(a => a.id === participante.adicional).valor).toFixed(2) }}</p>
    <v-text-field
      label="Nome"
      v-model="participante.nome"
      variant="outlined"
    ></v-text-field>
    <v-radio-group
      label="Adicinais"
      v-model="participante.adicional"
    >
      <v-radio
        v-for="adicional in adicionais"
        :label="`${adicional.nome} (R$ ${adicional.valor})`"
        :key="adicional.id"
        :value="adicional.id"
      ></v-radio>
    </v-radio-group>
    <!-- <v-text-field
      label="Adicional"
      v-model="participante.adicional"
      variant="outlined"
    ></v-text-field>
    <v-text-field
      label="Valor"
      v-model="participante.valor"
      variant="outlined"
    ></v-text-field> -->
  </div>
</template>

<script setup>
import { v4 as uuidv4 } from "uuid";
import { computed, ref } from "vue";

const somaAdicional = computed(() => {
  return participantes.value
    .map((participante) =>
      Object.assign(participante, {
        adicionalObjeto: adicionais.value.find(
          (adicional) => adicional.id === participante.adicional
        ),
      })
    )
    .map((p) => p.adicionalObjeto.valor)
    .reduce((a, b) => a + b, 0);
});

const totalParaCada = computed(() => {
  return (totalCombo.value - somaAdicional.value) / participantes.value.length;
});

const totalCombo = ref(0);
const participantes = ref([]);
const adicionais = ref([
  {
    id: 1,
    nome: "leite em pó",
    valor: 2,
  },
  {
    id: 2,

    nome: "paçoca",
    valor: 0,
  },
  {
    id: 3,

    nome: "leite condensado",
    valor: 0,
  },
  {
    id: 4,

    nome: "puro",
    valor: 0,
  },
  {
    id: 5,

    nome: "creme de cupuaçú",
    valor: 0,
  },
  {
    id: 6,

    nome: "ovomaltine",
    valor: 1.5,
  },
]);

function addParticipante() {
  participantes.value.push({
    id: uuidv4(),
    nome: "Participante " + (participantes.value.length + 1),
    adicional: 4,
    valor: 0,
  });
}
</script>
