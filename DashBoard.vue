<template>
  <main class="w-full flex-col bg-black">
    <section
      class="h-full p-6 grid grid-cols-[55%_45%] auto-rows-auto gap-5 flex-1 max-lg:grid-cols-1"
    >
      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg flex flex-col gap-4"
      >
        <div class="flex items-center">
          <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
          <h3 class="text-sm font-semibold">Traffic Overview</h3>
        </div>
        <div class="grid grid-cols-3 gap-4 flex-1">
          <div class="bg-slate-950 border border-slate-700 rounded-2xl p-4">
            <div class="flex items-center">
              <div class="w-1 h-4 bg-green-500 mr-2 rounded"></div>
              <h4 class="text-xs font-semibold">Allowed Traffic</h4>
            </div>
            <span class="text-[#BFC9D1] flex pt-14">No.</span>
          </div>

          <div class="bg-slate-950 border border-slate-700 rounded-2xl p-4">
            <div class="flex items-center">
              <div class="w-1 h-4 bg-red-500 mr-2 rounded"></div>
              <h4 class="text-xs font-semibold">Blocked Traffic</h4>
            </div>
            <span class="text-[#BFC9D1] flex pt-14">No.</span>
          </div>

          <div class="bg-slate-950 border border-slate-700 rounded-2xl p-4">
            <div class="flex items-center">
              <div class="w-1 h-4 bg-yellow-500 mr-2 rounded"></div>
              <h4 class="text-xs font-semibold">Throttled Traffic</h4>
            </div>
            <span class="text-[#BFC9D1] flex pt-14">No.</span>
          </div>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
      >
        <div class="flex items-center gap-2 mb-4">
          <div class="w-1 h-4 bg-blue-500 rounded"></div>
          <h3 class="text-sm font-semibold">Traffic Distribution</h3>
        </div>

        <div class="grid grid-cols-[45%_55%] items-center gap-6">
          <div class="relative flex justify-center">
            <canvas id="trafficGauge" class="w-[180px] h-[90px]"></canvas>

            <div class="absolute bottom-4 text-center">
              <p class="text-xl font-semibold text-blue-400">25%</p>
              <p class="text-xs text-green-400">Medium</p>
            </div>

            <div
              class="absolute bottom-0 w-full flex justify-between text-[11px] text-slate-400 px-1"
            >
              <span>0</span>
              <span>25</span>
              <span>50</span>
              <span>75</span>
              <span>100</span>
            </div>
          </div>

          <div class="flex flex-col gap-4 ml-10">
            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-slate-400">Benign</span>
                <span class="mr-20">60%</span>
              </div>
              <div class="w-40 h-2 bg-slate-800 rounded-full overflow-hidden">
                <div
                  class="h-full bg-blue-500 rounded-full"
                  style="width: 60%"
                ></div>
              </div>
            </div>

            <div>
              <div class="flex justify-between text-xs mb-1">
                <span class="text-slate-400">Suspicious</span>
                <span class="mr-20">40%</span>
              </div>
              <div class="w-40 h-2 bg-slate-800 rounded-full overflow-hidden">
                <div
                  class="h-full bg-blue-600 rounded-full"
                  style="width: 40%"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-4 shadow-lg flex flex-col gap-4"
      >
        <div class="flex justify-between items-center">
          <div class="flex items-center">
            <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
            <h3 class="text-sm font-semibold">Alerts</h3>
          </div>
          <select
            id="time-filter"
            class="text-xs bg-slate-800 border border-slate-700 px-3 py-1 rounded-full text-white outline-none cursor-pointer"
          >
            <option value="1">Last 1h</option>
            <option value="6">Last 6h</option>
            <option value="24" selected>Last 24h</option>
            <option value="168">Last 7 days</option>
            <option value="custom">Custom</option>
          </select>
        </div>

        <div class="bg-slate-950 rounded-2xl p-4 flex-1 overflow-hidden">
          <div class="grid grid-cols-6 text-xs text-slate-400 font-medium pb-3">
            <span>No.</span>
            <span>Time</span>
            <span>Source IP</span>
            <span>Destination</span>
            <span>Threat Type</span>
            <span>Severity</span>
          </div>

          <div id="packet-container" class="space-y-2 p-2"></div>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
      >
        <div class="flex justify-between items-center mb-4">
          <div class="flex items-center">
            <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
            <h3 class="text-sm font-semibold">Threat Distribution</h3>
          </div>
        </div>

        <div class="h-48">
          <canvas id="threatBarChart"></canvas>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
      >
        <div class="flex items-center">
          <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
          <h3 class="text-sm font-semibold">RL Agent Performance</h3>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
      >
        <!-- Header -->
        <div class="flex items-center mb-4">
          <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
          <h3 class="text-sm font-semibold">Threat Distribution</h3>
        </div>

        <!-- Chart container -->
        <div class="h-48 w-full">
          <canvas id="myChart"></canvas>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
      >
        <div class="flex justify-between items-center">
          <div class="flex items-center">
            <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
            <h3 class="text-sm font-semibold">Top 5 IPs with Most Traffic</h3>
          </div>

          <select
            class="text-xs bg-slate-800 border border-slate-700 px-3 py-1 rounded-full"
          >
            <option>Based on</option>
            <option>Based on</option>
            <option>Based on</option>
            <option>Based on</option>
            <option selected>Default</option>
          </select>
        </div>
        <div class="bg-slate-950 rounded-2xl overflow-hidden mt-4">
          <!-- Header with border -->
          <div
            class="grid grid-cols-3 text-xs text-slate-400 font-medium px-6 py-3 border-b border-slate-800"
          >
            <span>No.</span>
            <span>IP Address</span>
            <span>Risk Score</span>
          </div>

          <div class="space-y-2 p-2">
            <div
              class="grid grid-cols-3 items-center text-slate-200 bg-slate-900/40 rounded-lg px-6 py-3"
            >
              <span class="font-medium">
                <input type="radio" class="mr-1" />1</span
              >
              <span class="font-mono">192.168.1.12</span>
              <span class="text-red-400 font-semibold flex items-center">
                <div
                  class="w-2 h-2 bg-red-500 rounded-full mr-2 animate-pulse"
                ></div>
                8.7
              </span>
            </div>

            <div
              class="grid grid-cols-3 items-center text-slate-200 bg-slate-900/40 rounded-lg px-6 py-3"
            >
              <span class="font-medium">
                <input type="radio" class="mr-1" />2</span
              >
              <span class="font-mono">10.0.0.5</span>
              <span class="text-yellow-400 font-semibold flex items-center">
                <div
                  class="w-2 h-2 bg-yellow-500 rounded-full mr-2 animate-pulse"
                ></div>
                7.0
              </span>
            </div>

            <div
              class="grid grid-cols-3 items-center text-slate-200 bg-slate-900/40 rounded-lg px-6 py-3"
            >
              <span class="font-medium">
                <input type="radio" class="mr-1" />3</span
              >
              <span class="font-mono">172.16.0.23</span>
              <span class="text-green-400 font-semibold flex items-center">
                <div
                  class="w-2 h-2 bg-green-500 rounded-full mr-2 animate-pulse"
                ></div>
                3.2
              </span>
            </div>
            <div
              class="grid grid-cols-3 items-center text-slate-200 bg-slate-900/40 rounded-lg px-6 py-3"
            >
              <span class="font-medium">
                <input type="radio" class="mr-1" />4</span
              >
              <span class="font-mono">10.0.0.5</span>
              <span class="text-yellow-400 font-semibold flex items-center">
                <div
                  class="w-2 h-2 bg-yellow-500 rounded-full mr-2 animate-pulse"
                ></div>
                7.0
              </span>
            </div>

            <div
              class="grid grid-cols-3 items-center text-slate-200 bg-slate-900/40 rounded-lg px-6 py-3"
            >
              <span class="font-medium">
                <input type="radio" class="mr-1" />5</span
              >
              <span class="font-mono">172.16.0.23</span>
              <span class="text-green-400 font-semibold flex items-center">
                <div
                  class="w-2 h-2 bg-green-500 rounded-full mr-2 animate-pulse"
                ></div>
                3.2
              </span>
            </div>
          </div>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-4 shadow-lg"
      >
        <div class="flex items-center pb-2">
          <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
          <h3 class="text-sm font-semibold">System Health</h3>
        </div>

        <div class="bg-slate-950 border border-slate-800 rounded-2xl p-4">
          <div class="grid grid-cols-3 gap-6 text-center">
            <div class="relative flex flex-col items-center">
              <canvas id="SysGauge1" class="w-[80px] h-[40px]"></canvas>
              <p class="mt-2 text-sm font-semibold text-blue-400">25%</p>
              <p class="text-xs text-slate-400">CPU</p>
            </div>

            <div class="relative flex flex-col items-center">
              <canvas id="SysGauge2" class="w-[80px] h-[40px]"></canvas>
              <p class="mt-2 text-sm font-semibold text-blue-400">48%</p>
              <p class="text-xs text-slate-400">Memory</p>
            </div>

            <div class="relative flex flex-col items-center">
              <canvas id="SysGauge3" class="w-[80px] h-[40px]"></canvas>
              <p class="mt-2 text-sm font-semibold text-blue-400">67%</p>
              <p class="text-xs text-slate-400">Local Disk</p>
            </div>
          </div>
        </div>
      </div>

      <div
        class="bg-[#181B2C] border border-slate-800 rounded-2xl p-4 shadow-lg"
      >
        <div class="flex items-center pb-2">
          <div class="w-1 h-4 bg-blue-500 mr-2 rounded"></div>
          <h3 class="text-sm font-semibold">Network Health</h3>
        </div>

        <div
          class="bg-slate-950 border border-slate-800 rounded-2xl p-4 h-72 w-1/2 justify-center m-auto"
        >
          <canvas id="NetChart"></canvas>
        </div>
      </div>
    </section>
  </main>
</template>

<!-- <script>
document.addEventListener("DOMContentLoaded", () => {
  initDurationSelectors();
  initNotifications();
});

function initDurationSelectors() {
  const selectors = document.querySelectorAll("select");

  selectors.forEach((select) => {
    select.addEventListener("change", (e) => {
      const duration = e.target.value;
      console.log("Selected duration:", duration);
    });
  });
}

let notificationCount = 3;

function initNotifications() {
  const badge = document.querySelector(".notification-badge");

  if (!badge) return;

  badge.textContent = notificationCount;

  setTimeout(() => {
    incrementNotification();
  }, 5000);
}

function incrementNotification() {
  notificationCount++;
  const badge = document.querySelector(".notification-badge");
  badge.textContent = notificationCount;
}

const threatCtx = document.getElementById("threatBarChart");

// Function to create chart with percentages
function createPercentageBarChart(ctx, labels, rawValues, colors) {
  // Calculate percentages
  const total = rawValues.reduce((a, b) => a + b, 0);
  const percentages = rawValues.map((val) => ((val / total) * 100).toFixed(1));

  return new Chart(ctx, {
    type: "bar",
    data: {
      labels: labels,
      datasets: [
        {
          data: percentages,
          backgroundColor: colors,
          borderRadius: 6,
          barThickness: 18,
          borderWidth: 0,
        },
      ],
    },
    options: {
      indexAxis: "y",
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: "rgba(30, 41, 59, 0.9)",
          titleColor: "#f1f5f9",
          bodyColor: "#cbd5e1",
          borderColor: "#475569",
          borderWidth: 1,
          callbacks: {
            label: (context) => {
              const rawVal = rawValues[context.dataIndex];
              const percent = context.parsed.x;
              return `${percent}% (${rawVal} incidents)`;
            },
          },
        },
      },
      scales: {
        x: {
          beginAtZero: true,
          max: 100,
          grid: {
            color: "rgba(148,163,184,0.1)",
            drawTicks: false,
          },
          ticks: {
            color: "#94a3b8",
            callback: (value) => `${value}%`,
            maxTicksLimit: 5,
            padding: 8,
          },
          border: {
            display: false,
          },
        },
        y: {
          grid: {
            display: false,
          },
          ticks: {
            color: "#e5e7eb",
            font: {
              size: 12,
              weight: "500",
              family: "'Inter', sans-serif",
            },
            padding: 12,
          },
          border: {
            display: false,
          },
        },
      },
      animation: {
        duration: 800,
      },
    },
  });
}

// Usage
let threatChart = createPercentageBarChart(
  threatCtx,
  ["Malware", "DDoS", "Port Scan"],
  [81, 28, 17],
  ["#3b82f6", "#3b82f6", "#3b82f6"],
);

const ctx = document.getElementById("trafficGauge");

new Chart(ctx, {
  type: "doughnut",
  data: {
    datasets: [
      {
        data: [25, 75],
        backgroundColor: ["#3b82f6", "#1e293b"],
        borderWidth: 0,
      },
    ],
  },
  options: {
    rotation: -90,
    circumference: 180,
    cutout: "75%",
    responsive: true,
    plugins: {
      legend: { display: false },
      tooltip: { enabled: false },
    },
  },
});

function createHalfGauge(canvasId, value, color) {
  new Chart(document.getElementById(canvasId), {
    type: "doughnut",
    data: {
      datasets: [
        {
          data: [value, 100 - value],
          backgroundColor: [color, "#1e293b"],
          borderWidth: 0,
        },
      ],
    },
    options: {
      rotation: -90,
      circumference: 180,
      cutout: "75%",
      responsive: true,
      plugins: {
        legend: { display: false },
        tooltip: { enabled: false },
      },
    },
  });
}

/* Create charts */
createHalfGauge("SysGauge1", 25, "#3b82f6");
createHalfGauge("SysGauge2", 48, "#3b82f6");
createHalfGauge("SysGauge3", 67, "#3b82f6");

const MulChart = document.getElementById("myChart");

new Chart(MulChart, {
  type: "line",
  data: {
    labels: Array(10).fill(""),
    datasets: [
      {
        data: [1600, 1700, 1700, 1900, 2000, 2700, 4000, 5000, 6000, 7000],
        borderColor: "#22c55e",
        tension: 0.4,
        pointRadius: 0,
      },
      {
        data: [1000, 1240, 1500, 1100, 2000, 2400, 4300, 4000, 2100, 7000],
        borderColor: "#c53b22",
        tension: 0.4,
        pointRadius: 0,
      },
      {
        data: [300, 700, 2000, 5000, 6000, 4000, 2000, 1000, 200, 100],
        borderColor: "#3b82f6",
        tension: 0.4,
        pointRadius: 0,
      },
    ],
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { display: false },
      tooltip: { enabled: true },
    },
    scales: {
      x: {
        display: false,
        grid: { display: false },
      },
      y: {
        display: true,
        grid: {
          color: "rgba(148,163,184,0.15)",
        },
        ticks: {
          color: "#94a3b8",
          font: { size: 10 },
        },
      },
    },
  },
});

const netCtx = document.getElementById("NetChart");

new Chart(netCtx, {
  type: "doughnut",
  data: {
    labels: ["Throughput", "Latence", "Errors"],
    datasets: [
      {
        data: [65, 25, 10],
        backgroundColor: [
          "#22c55e", // green
          "#facc15", // yellow
          "#ef4444", // red
        ],
        borderWidth: 0,
      },
    ],
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    cutout: "70%",
    plugins: {
      legend: {
        position: "bottom",
        labels: {
          color: "#94a3b8",
          font: { size: 11 },
        },
      },
      tooltip: {
        enabled: true,
      },
    },
  },
});
</script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script> -->
