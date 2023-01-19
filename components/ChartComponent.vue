<script setup>
import Chart from "chart.js/auto";
import { onMounted } from "vue";
import * as Utils from "./Utils";
onMounted(() => {
  const horizontalDottedLine = {
    id: "horizontalDottedLine",
    beforeDatasetsDraw(chart, args, options) {
      const {
        ctx,
        chartArea: { top, right, bottom, left, width, height },
        scales: { x, y },
      } = chart;
      ctx.save();

      ctx.strokeStyle = "grey";
      ctx.setLineDash([10, 5]);
      ctx.strokeRect(0, y.getPixelForValue(2.5), width + 100, 0);
    },
  };

  let chart = new Chart(document.getElementById("myChart"), {
    type: "bar",

    data: {
      labels: [
        "CURRENT MO.",
        "NEXT MO.",
        "FOLLOWING MO.",
        "CURRENT Q",
        "NEXT Q",
      ],

      datasets: [
        {
          label: "CERTAIN",
          backgroundColor: [
            "#77b9e5",
            "#77b9e5",
            "#77b9e5",
            "#77b9e5",
            "#77b9e5",
          ],
          barPercentage: 0.5,
          data: [
            [7500000, 8100000],
            [7000000, 7500000],
            [7200000, 7800000],
            [6800000, 7600000],
            [7000000, 7500000],
          ],
        },
        {
          label: "EXPECTED",
          backgroundColor: [
            "#e7c81c",
            "#e7c81c",
            "#e7c81c",
            "#e7c81c",
            "#e7c81c",
          ],
          barPercentage: 0.5,

          data: [
            [8000000, 8700000],
            [7500000, 8700000],
            [7800000, 8700000],
            [7600000, 8400000],
            [7500000, 8200000],
          ],
        },
        {
          xAxisID: "A",
          label: "UNLIKELY",
          backgroundColor: [
            "#ee3f37",
            "#ee3f37",
            "#ee3f37",
            "#ee3f37",
            "#ee3f37",
          ],
          barPercentage: 0.5,

          data: [
            [8670000, 9000000],
            [8670000, 9400000],
            [8670000, 9200000],
            [8400000, 9000000],
            [8200000, 9000000],
          ],
        },
      ],
    },

    plugins: [horizontalDottedLine],
    options: {
      indexAxis: "y",
      responsive: true,
      scales: {
        A: {
          position: "top",
          grid: { display: false },
          border: {
            display: false,
          },
          min: 6500000,
          max: 9500000,
          ticks: {
            callback: (value) => {
              let val = value - 6000000;
              let values =
                val.toString().length < 7
                  ? "$0." + val.toFixed(2) / 100000 + "M"
                  : "$" +
                    val.toString().substring(0, 2).split("").join(".") +
                    "M";

              //   .toString()
              //   .substring(0, 2)
              //   .split("")
              //   .join(".");
              return values;
            },
          },
        },
        x: {
          border: {
            display: false,
          },
          grid: {
            lineWidth: 1,
            color: "black",
          },
          min: 6500000,
          max: 9500000,

          ticks: {
            callback: (value) => {
              let val = value.toString().substring(0, 2).split("").join(".");
              return "$" + val + "M";
            },
          },
        },
        y: {
          ticks: {
            crossAlign: "far",
          },
          border: {
            display: false,
          },
          grid: {
            borderWidth: 0,
            lineWidth: 0,
          },
          stacked: true,
        },
      },
      elements: {
        bar: {
          borderColor: "white",
          hoverBorderColor: "white",
        },
      },
      plugins: {
        legend: {
          display: false,
        },

        tooltip: { enabled: false },
      },
    },
  });
  return function toggleData(value) {
    const visibilityData = chart.isDatasetVisible(value);
    if (visibilityData === true) {
      chart.hide(value);
      if ((visibilityData = false)) {
        chart.show(value);
      }
    }
  };
});
</script>
<template>
  <div>
    <!-- <div class="title"></div> -->
    <div class="buttons">
      <h2>Where are we going to land?</h2>
      <button
        @click="toggleData(0)"
        class="certain"
        style="background-color: #77b9e5"
      >
        CERTAIN
      </button>
      <button
        @click="toggleData(1)"
        class="expected"
        style="background-color: #e7c81c"
      >
        EXPECTED
      </button>
      <button
        @click="toggleData(2)"
        class="unlikely"
        style="background-color: #ee3f37"
      >
        UNLIKELY
      </button>
    </div>
    <div class="container">
      <canvas id="myChart"></canvas>
    </div>
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.buttons {
  display: flex;
  justify-content: left;
  align-items: center;
  margin-bottom: 2rem;
}

button {
  margin: 0 0.3rem 0.7rem 0.3rem;
  font-size: 12px;
  border: none;
  outline: none;
  padding: 1px 15px;
  height: 1.1rem;
  border-radius: 10px;
}

.certain {
  color: white;
  margin-left: 1rem;
}
.unlikely {
  color: white;
}

#myChart {
  width: 65rem;
  height: 400px !important;
}
</style>
