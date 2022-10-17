<template>
  <div style="text-align: center">
    <h1>Convert Dollar to GCC Correspondant</h1>
    <input
      type="number"
      v-model="inDollar"
      placeholder="Enter amount in Dollar"
    />
    <div class="container">
      <div
        class="gcc-curreny-card"
        v-for="symbol in gccSymbols"
        :key="symbol.label"
      >
        {{ symbol.country }}
        <div class="currency-value">
          {{ gccCurrencyValue[symbol.label] + " " + symbol.label }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//Curreny rate

let customHeaders = new Headers();
customHeaders.append("apikey", "G1hLzz5zBX5JWYotF52BmJ0aLjviF38e");

const requestOptions = {
  method: "GET",
  redirect: "follow",
  headers: customHeaders,
};

export default {
  name: "App",
  data() {
    return {
      inDollar: 0,
      dollarSymbols: {
        country: "United States Dollar",
        label: "USD",
      },
      gccCurrencyValue: {
        AED: 0,
        BHD: 0,
        KWD: 0,
        OMR: 0,
        QAR: 0,
        SAR: 0,
      },
      gccSymbols: [
        {
          country: "United Arab Emirates Dirham",
          label: "AED",
        },
        {
          country: "Bahraini Dinar",
          label: "BHD",
        },
        {
          country: "Kuwaiti Dinar",
          label: "KWD",
        },
        {
          country: "Omani Rial",
          label: "OMR",
        },
        {
          country: "Qatari Rial",
          label: "QAR",
        },
        {
          country: "Saudi Riyal",
          label: "SAR",
        },
      ],
    };
  },
  watch: {
    inDollar(val) {
      this.loadCurrencyValue(val);
    },
  },
  methods: {
    loadCurrencyValue(amount) {
      if (amount > 0) {
        this.gccSymbols.forEach((item) => {
          fetch(
            `https://api.apilayer.com/exchangerates_data/convert?to=${item.label}&from=USD&amount=${amount}`,
            requestOptions
          )
            .then((response) => response.json())

            .then((data) => {
              this.gccCurrencyValue[item.label] = data.result;
              console.log(data);
            })
            .catch((error) => console.error("error", error));
        });
      }
    },
  },
};
</script>

<style>
.gcc-curreny-card {
  background-color: #fafafa;
  width: 180px;
  height: 84px;
  font-family: monospace;
  margin: 8px;
  padding: 12px;
  border-radius: 8px;
  box-shadow: 0px 0px 5px 0px rgb(174 174 174 / 47%);
  -webkit-box-shadow: 0px 0px 5px 0px rgb(174 174 174 / 47%);
  -moz-box-shadow: 0px 0px 5px 0px rgb(174 174 174 / 47%);
}
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 18px;
}
.currency-value {
  font-size: 18px;
  margin-top: 12px;
}
</style>
