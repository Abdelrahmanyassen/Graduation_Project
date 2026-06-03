<template>
  <main class="bg-black text-slate-200 min-h-screen flex">
    <!-- Sidebar -->

    <main class="w-full flex-col bg-black">
      <section class="p-6 grid grid-cols-1 gap-5">
        <!-- Packet Capture -->
        <div class="bg-white border border-slate-800 rounded-2xl p-6 shadow-sm">
          <div class="flex items-center gap-3">
            <div class="w-1 h-6 bg-blue-500 rounded"></div>
            <h1 class="font-bold text-black tracking-wide">Packet Capture</h1>
          </div>
        </div>

        <!-- Search -->
        <div
          class="bg-[#2D2D2D] border border-slate-800 rounded-2xl p-5 shadow-inner"
        >
          <input
            id="packet-search"
            type="text"
            placeholder="Search by IP, Protocol, or Threat..."
            class="bg-[#1f1f1f] w-full h-9 px-4 rounded-xl text-sm text-slate-300 placeholder-slate-500 focus:outline-none focus:ring-1 focus:ring-blue-500"
          />
        </div>

        <!-- Packet Table -->
        <div
          class="bg-[#181B2C] border border-slate-800 rounded-2xl p-5 shadow-lg"
        >
          <div class="bg-slate-950 rounded-2xl overflow-hidden">
            <div
              class="grid grid-cols-6 text-xs text-slate-500 bg-white font-semibold px-6 py-3 border-b border-slate-800"
            >
              <span>No.</span>
              <span>Source IP</span>
              <span>Destination IP</span>
              <span>Protocol</span>
              <span>Time</span>
              <span>Packet Size</span>
            </div>

            <div id="packet-container" class="space-y-2 p-3"></div>
          </div>
        </div>

        <!-- Details + Chart -->
        <div class="grid grid-cols-[60%_40%] gap-5">
          <!-- Packet Details -->
          <div
            class="bg-slate-950 border border-slate-800 rounded-2xl p-6 shadow-lg"
          >
            <div class="flex gap-3">
              <div class="w-1 h-6 bg-blue-500 rounded"></div>
              <div class="w-full text-start">
                <h1 class="font-bold text-white text-sm">Packet Details</h1>
                <div class="mt-4 space-y-2 text-xs text-slate-400 font-mono">
                  <p>
                    Source IP: <span id="det-src" class="text-blue-400">-</span>
                  </p>
                  <p>
                    Destination IP:
                    <span id="det-dest" class="text-blue-400">-</span>
                  </p>
                  <p>
                    Protocol:
                    <span id="det-proto" class="text-blue-400">-</span>
                  </p>
                  <p>
                    Size: <span id="det-size" class="text-blue-400">-</span>
                  </p>
                </div>
              </div>
            </div>
          </div>

          <!-- Traffic Trend -->
          <div
            class="bg-slate-950 border border-slate-800 rounded-2xl p-6 shadow-lg flex flex-col"
          >
            <div class="flex items-center gap-3 mb-4">
              <div class="w-1 h-6 bg-blue-500 rounded"></div>
              <h1 class="font-bold text-white">Traffic Trend</h1>
            </div>

            <div class="flex-1 relative min-h-[220px]">
              <canvas id="TrafficTrend" class="w-full h-full"></canvas>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="./JS/traffic.js"></script> -->
  </main>
</template>
