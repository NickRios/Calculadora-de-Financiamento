<template>
  <section class="section">
    <form class="container-admin">
      <h3 class="title is-3">Cálculo de Financiamento</h3>

      <h3 class="subtitle is-6" style="color: darkgrey">
        Para iniciar a simulação de financiamento, informe os valores abaixo:
      </h3>

      <div class="columns fields-content">
        <div class="column is-one-third">
          <b-field label="Valor do Financiamento">
            <b-input
              v-model="form.financingAmount"
              placeholder="0,00"
              icon-right="cash"
              type="number"
              required
            />
          </b-field>
        </div>
        <div class="column is-one-third">
          <b-field label="Quantidade de períodos">
            <b-input
              type="number"
              v-model="form.periods"
              placeholder="0"
              icon-right="clock-outline"
              required
            />
          </b-field>
        </div>
        <div class="column is-one-third">
          <b-field label="Taxa de Juros">
            <b-input
              type="number"
              v-model="form.rate"
              placeholder="0"
              icon-right="percent"
              required
            />
          </b-field>
        </div>
      </div>
      <div class="columns is-multiline">
        <div class="column is-full">
          <h3 class="title is-6">Selecione um tipo de simulação</h3>
        </div>
        <div class="column">
          <b-radio v-model="form.simulationType" native-value="SAC"> SAC </b-radio>
          <b-radio v-model="form.simulationType" native-value="PRICE"> PRICE </b-radio>
        </div>
        <div class="column is-full" style="margin-top: 10px">
          <b-button type="is-secondary" expanded> Simular Financiamento </b-button>
        </div>
      </div>
    </form>

    <div class="column" style="margin-top: 3.5rem">
      <h3 class="title is-4">{{ form.simulationType }}</h3>
    </div>

    <div class="table-content">
      <b-table hoverable mobile-cards>
        <b-table-column field="periodo" label="Período" v-slot="props">
          {{ props.row.periodo }}
        </b-table-column>

        <b-table-column field="amortizacao" label="Amortização" v-slot="props" centered>
          {{ props.row.amortizacao }}
        </b-table-column>

        <b-table-column field="juros" label="Juros" v-slot="props" centered>
          {{ props.row.juros }}
        </b-table-column>

        <b-table-column field="prestacao" label="Prestação" v-slot="props" centered>
          {{ props.row.prestacao }}
        </b-table-column>

        <b-table-column field="valorPago" label="Valor Pago" v-slot="props" centered>
          {{ props.row.valorPago }}
        </b-table-column>

        <b-table-column field="saldoDevedor" label="Saldo Devedor" v-slot="props" centered>
          {{ props.row.saldoDevedor }}
        </b-table-column>
      </b-table>
    </div>
  </section>
</template>

<script>
import sac from "@/services/sac";

export default {
  name: "Home",
  data() {
    return {
      form: {},
    };
  },
  methods: {},
};
</script>

<style scoped>
.fields-content {
  margin-top: 2rem;
  justify-content: center;
}
.table-content {
  margin-top: 1.5rem;
}
</style>
