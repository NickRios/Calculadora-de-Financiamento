<template>
  <section class="section">
    <h3 class="title is-3">Cálculo de Financiamento</h3>

    <h3 class="subtitle is-6" style="color: darkgrey">
      Para iniciar a simulação de financiamento, informe os valores abaixo:
    </h3>

    <div class="columns fields-content">
      <div class="column is-one-third">
        <b-field label="Valor do Financiamento">
          <b-input v-model="value" placeholder="R$ 0,00" icon-right="cash" type="number" required />
        </b-field>
      </div>
      <div class="column is-one-third">
        <b-field label="Quantidade de períodos">
          <b-input
            type="number"
            v-model="periods"
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
            v-model="interestRate"
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
      <div class="column is-full">
        <b-radio v-model="simulationType" native-value="SAC"> SAC </b-radio>
        <b-radio v-model="simulationType" native-value="PRICE"> PRICE </b-radio>
      </div>
      <div class="column is-half" style="margin-top: 10px">
        <b-button
          type="is-secondary"
          tag="input"
          native-type="submit"
          value=" Simular Financiamento"
          expanded
          @click="handleSimulation"
        />
      </div>
      <div class="column is-half" style="margin-top: 10px">
        <b-button
          type="is-danger"
          tag="input"
          native-type="submit"
          value="Limpar Financiamento"
          expanded
          @click="clearValues"
        />
      </div>
    </div>

    <div class="column" style="margin-top: 2.5rem">
      <h3 class="title is-4">{{ simulationType }}</h3>
    </div>

    <div v-if="simulationType === 'SAC'" class="table-content">
      <b-table :data="sac.prestacoes" :key="sac.index" hoverable mobile-cards>
        <b-table-column field="index" label="Período" v-slot="props">
          {{ props.row.index }}
        </b-table-column>

        <b-table-column field="amortization" label="Amortização" v-slot="props" centered>
          {{ props.row.amortization }}
        </b-table-column>

        <b-table-column field="interestRateBalance" label="Juros" v-slot="props" centered>
          {{ props.row.interestRateBalance }}
        </b-table-column>

        <b-table-column field="portion" label="Prestação" v-slot="props" centered>
          {{ props.row.portion }}
        </b-table-column>

        <b-table-column field="valuePaid" label="Valor Pago" v-slot="props" centered>
          {{ props.row.valuePaid }}
        </b-table-column>

        <b-table-column field="balance" label="Saldo Devedor" v-slot="props" centered>
          {{ props.row.balance }}
        </b-table-column>
      </b-table>
    </div>

    <div v-if="simulationType === 'PRICE'">
      <hr size="50" />
      <div class="columns is-multiline">
        <div class="column is-one-third">
          <div class="detail-price">
            <h3 class="title is-5">Valor da Prestação</h3>
            <p>{{ price.valorPrestacao }}</p>
          </div>
        </div>
        <div class="column is-one-third">
          <div class="detail-price">
            <h3 class="title is-5">Valor total do Financiamento</h3>
            <p>{{ price.valorTotalFinanciamento }}</p>
          </div>
        </div>
        <div class="column is-one-third">
          <div class="detail-price">
            <h3 class="title is-5">Valor total de Juros pago</h3>
            <p>{{ price.valorTotalJuros }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { Financiamento } from "@/services/api";

export default {
  name: "Home",
  data() {
    return {
      value: null,
      periods: null,
      interestRate: null,
      sac: null,
      price: null,
      simulationType: "",
    };
  },
  methods: {
    handleSac() {
      const sac = new Financiamento(this.value, this.interestRate, this.periods);
      sac.formatarDados();

      this.sac = sac.financiarSac();
    },
    handlePrice() {
      const price = new Financiamento(this.value, this.interestRate, this.periods);

      price.formatarDados();
      this.price = price.financiarPrice();
    },
    handleSimulation() {
      if (this.simulationType === "SAC") this.handleSac();
      else if (this.simulationType === "PRICE") this.handlePrice();
    },
    clearValues() {
      this.sac = null;
      this.price = null;
      this.value = null;
      this.interestRate = null;
      this.periods = null;
      this.simulationType = "";
    },
  },
};
</script>

<style lang="scss">
.section {
  padding: 2rem 1.5rem;

  .fields-content {
    margin-top: 2rem;
    justify-content: center;
  }

  .table-content {
    margin-top: 1.5rem;
  }

  .detail-price {
    margin-top: 30px;
  }
}
</style>
