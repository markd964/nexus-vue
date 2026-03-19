<template>
  <div class="crypto-page">

    <!-- Page Hero -->
    <div class="page-hero">
      <div class="container">
        <div class="hero-top">
          <div>
            <p class="section-label">/ Crypto Wallet</p>
            <h1 class="page-title">Your <span>Portfolio</span></h1>
          </div>
          <div class="hero-actions">
            <button class="btn-outline" @click="showModal('receive')">
              <span class="btn-icon">↓</span> Receive
            </button>
            <button class="btn-primary" @click="showModal('send')">
              <span class="btn-icon">↑</span> Send
            </button>
          </div>
        </div>
        <!-- Total Balance Card -->
        <div class="balance-card">
          <div class="balance-bg"></div>
          <div class="balance-inner">
            <div class="balance-left">
              <p class="balance-label">Total Portfolio Value</p>
              <p class="balance-amount">
                ${{ formatUSD(totalValue) }}
              </p>
              <div class="balance-change" :class="totalChange >= 0 ? 'positive' : 'negative'">
                <span class="change-arrow">{{ totalChange >= 0 ? '▲' : '▼' }}</span>
                {{ Math.abs(totalChange).toFixed(2) }}%
                <span class="change-period">24h</span>
              </div>
            </div>
            <div class="balance-right">
              <div class="mini-chart">
                <svg viewBox="0 0 200 60" preserveAspectRatio="none">
                  <defs>
                    <linearGradient id="chartGrad" x1="0" y1="0" x2="0" y2="1">
                      <stop offset="0%" stop-color="#c8f53b" stop-opacity="0.3"/>
                      <stop offset="100%" stop-color="#c8f53b" stop-opacity="0"/>
                    </linearGradient>
                  </defs>
                  <path :d="chartAreaPath" fill="url(#chartGrad)"/>
                  <path :d="chartLinePath" fill="none" stroke="#c8f53b" stroke-width="2" stroke-linecap="round"/>
                </svg>
              </div>
              <p class="chart-label">7-day trend</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="container main-grid">

      <!-- LEFT COLUMN -->
      <div class="left-col">

        <!-- Wallet Assets -->
        <div class="panel">
          <div class="panel-header">
            <h2 class="panel-title">Assets</h2>
            <div class="view-toggle">
              <button :class="{ active: assetView === 'list' }" @click="assetView = 'list'">List</button>
              <button :class="{ active: assetView === 'grid' }" @click="assetView = 'grid'">Grid</button>
            </div>
          </div>

          <!-- List View -->
          <div v-if="assetView === 'list'" class="asset-list">
            <div
              class="asset-row"
              v-for="asset in walletAssets"
              :key="asset.symbol"
              @click="selectAsset(asset)"
              :class="{ selected: selectedAsset?.symbol === asset.symbol }"
            >
              <div class="asset-icon" :style="{ background: asset.color + '22', color: asset.color }">
                {{ asset.icon }}
              </div>
              <div class="asset-info">
                <p class="asset-name">{{ asset.name }}</p>
                <p class="asset-symbol">{{ asset.balance }} {{ asset.symbol }}</p>
              </div>
              <div class="asset-value">
                <p class="asset-usd">${{ formatUSD(asset.balance * asset.price) }}</p>
                <p class="asset-change" :class="asset.change24h >= 0 ? 'positive' : 'negative'">
                  {{ asset.change24h >= 0 ? '+' : '' }}{{ asset.change24h.toFixed(2) }}%
                </p>
              </div>
              <div class="asset-price-col">
                <p class="asset-price">${{ formatUSD(asset.price) }}</p>
                <div class="spark">
                  <svg viewBox="0 0 60 24" preserveAspectRatio="none">
                    <polyline
                      :points="sparkPoints(asset.spark)"
                      fill="none"
                      :stroke="asset.change24h >= 0 ? '#c8f53b' : '#ff3b3b'"
                      stroke-width="1.5"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    />
                  </svg>
                </div>
              </div>
            </div>
          </div>

          <!-- Grid View -->
          <div v-else class="asset-grid">
            <div
              class="asset-card"
              v-for="asset in walletAssets"
              :key="asset.symbol"
              @click="selectAsset(asset)"
              :class="{ selected: selectedAsset?.symbol === asset.symbol }"
              :style="{ '--ac': asset.color }"
            >
              <div class="ac-top">
                <div class="ac-icon" :style="{ background: asset.color + '22', color: asset.color }">{{ asset.icon }}</div>
                <span class="ac-change" :class="asset.change24h >= 0 ? 'positive' : 'negative'">
                  {{ asset.change24h >= 0 ? '+' : '' }}{{ asset.change24h.toFixed(2) }}%
                </span>
              </div>
              <p class="ac-name">{{ asset.name }}</p>
              <p class="ac-usd">${{ formatUSD(asset.balance * asset.price) }}</p>
              <p class="ac-bal">{{ asset.balance }} {{ asset.symbol }}</p>
            </div>
          </div>
        </div>

        <!-- Transaction History -->
        <div class="panel">
          <div class="panel-header">
            <h2 class="panel-title">Transactions</h2>
            <div class="tx-filters">
              <button
                v-for="f in txFilters"
                :key="f"
                :class="{ active: txFilter === f }"
                @click="txFilter = f"
              >{{ f }}</button>
            </div>
          </div>
          <div class="tx-list">
            <div class="tx-row" v-for="tx in filteredTx" :key="tx.id">
              <div class="tx-icon" :class="tx.type">
                {{ tx.type === 'received' ? '↓' : tx.type === 'sent' ? '↑' : '⇄' }}
              </div>
              <div class="tx-info">
                <p class="tx-title">{{ tx.title }}</p>
                <p class="tx-date">{{ tx.date }} · {{ tx.hash }}</p>
              </div>
              <div class="tx-amount" :class="tx.type">
                <p>{{ tx.type === 'received' ? '+' : tx.type === 'sent' ? '-' : '' }}{{ tx.amount }} {{ tx.symbol }}</p>
                <p class="tx-usd">${{ formatUSD(tx.usd) }}</p>
              </div>
              <div class="tx-status" :class="tx.status">{{ tx.status }}</div>
            </div>
          </div>
        </div>
      </div>

      <!-- RIGHT COLUMN -->
      <div class="right-col">

        <!-- Selected Asset Detail -->
        <div class="panel detail-panel" v-if="selectedAsset">
          <div class="detail-header">
            <div class="detail-icon" :style="{ background: selectedAsset.color + '22', color: selectedAsset.color }">
              {{ selectedAsset.icon }}
            </div>
            <div>
              <h3 class="detail-name">{{ selectedAsset.name }}</h3>
              <p class="detail-sym">{{ selectedAsset.symbol }}</p>
            </div>
            <div class="detail-price-block">
              <p class="detail-price">${{ formatUSD(selectedAsset.price) }}</p>
              <span class="detail-change" :class="selectedAsset.change24h >= 0 ? 'positive' : 'negative'">
                {{ selectedAsset.change24h >= 0 ? '▲' : '▼' }} {{ Math.abs(selectedAsset.change24h).toFixed(2) }}%
              </span>
            </div>
          </div>
          <div class="detail-stats">
            <div class="d-stat">
              <p class="ds-label">Holdings</p>
              <p class="ds-val">{{ selectedAsset.balance }} {{ selectedAsset.symbol }}</p>
            </div>
            <div class="d-stat">
              <p class="ds-label">Value (USD)</p>
              <p class="ds-val">${{ formatUSD(selectedAsset.balance * selectedAsset.price) }}</p>
            </div>
            <div class="d-stat">
              <p class="ds-label">Market Cap</p>
              <p class="ds-val">{{ selectedAsset.mktcap }}</p>
            </div>
            <div class="d-stat">
              <p class="ds-label">24h Volume</p>
              <p class="ds-val">{{ selectedAsset.vol }}</p>
            </div>
          </div>
          <div class="detail-chart">
            <div class="chart-tabs">
              <button
                v-for="r in chartRanges"
                :key="r"
                :class="{ active: chartRange === r }"
                @click="chartRange = r; generateChartData()"
              >{{ r }}</button>
            </div>
            <svg class="line-chart" viewBox="0 0 400 120" preserveAspectRatio="none">
              <defs>
                <linearGradient id="detailGrad" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" :stop-color="selectedAsset.color" stop-opacity="0.25"/>
                  <stop offset="100%" :stop-color="selectedAsset.color" stop-opacity="0"/>
                </linearGradient>
              </defs>
              <path :d="detailAreaPath" fill="url(#detailGrad)"/>
              <path :d="detailLinePath" fill="none" :stroke="selectedAsset.color" stroke-width="2" stroke-linecap="round"/>
              <!-- Price dot at end -->
              <circle :cx="detailEndX" :cy="detailEndY" r="4" :fill="selectedAsset.color"/>
            </svg>
          </div>
          <div class="detail-actions">
            <button class="btn-outline small" @click="showModal('send')">Send {{ selectedAsset.symbol }}</button>
            <button class="btn-primary small" @click="showModal('buy')">Buy More</button>
          </div>
        </div>

        <!-- Portfolio Allocation -->
        <div class="panel">
          <div class="panel-header">
            <h2 class="panel-title">Allocation</h2>
          </div>
          <div class="allocation-chart">
            <svg viewBox="0 0 160 160" class="donut-svg">
              <g transform="translate(80,80)">
                <circle r="50" fill="none" stroke="var(--gray-2)" stroke-width="22"/>
                <circle
                  v-for="(seg, i) in donutSegments"
                  :key="i"
                  r="50"
                  fill="none"
                  :stroke="seg.color"
                  stroke-width="22"
                  :stroke-dasharray="`${seg.dash} ${314 - seg.dash}`"
                  :stroke-dashoffset="-seg.offset"
                  stroke-linecap="butt"
                />
              </g>
            </svg>
            <div class="donut-legend">
              <div class="legend-item" v-for="asset in walletAssets" :key="asset.symbol">
                <span class="legend-dot" :style="{ background: asset.color }"></span>
                <span class="legend-name">{{ asset.symbol }}</span>
                <span class="legend-pct">{{ allocationPct(asset) }}%</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Market Watch -->
        <div class="panel">
          <div class="panel-header">
            <h2 class="panel-title">Market Watch</h2>
            <span class="live-badge"><span class="live-dot"></span> Live</span>
          </div>
          <div class="market-list">
            <div class="market-row" v-for="coin in marketCoins" :key="coin.symbol">
              <div class="mkt-icon" :style="{ background: coin.color + '22', color: coin.color }">{{ coin.icon }}</div>
              <div class="mkt-info">
                <p class="mkt-name">{{ coin.name }}</p>
                <p class="mkt-sym">{{ coin.symbol }}</p>
              </div>
              <div class="mkt-price-block">
                <p class="mkt-price">${{ formatUSD(coin.price) }}</p>
                <p class="mkt-change" :class="coin.change >= 0 ? 'positive' : 'negative'">
                  {{ coin.change >= 0 ? '+' : '' }}{{ coin.change.toFixed(2) }}%
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- MODALS -->
    <transition name="modal">
      <div class="modal-overlay" v-if="activeModal" @click.self="activeModal = null">
        <div class="modal-box">

          <!-- SEND MODAL -->
          <div v-if="activeModal === 'send'">
            <div class="modal-header">
              <h3>Send Crypto</h3>
              <button class="modal-close" @click="activeModal = null">✕</button>
            </div>
            <div class="modal-body">
              <div class="m-field">
                <label>Asset</label>
                <select class="m-input" v-model="sendForm.asset">
                  <option v-for="a in walletAssets" :key="a.symbol" :value="a.symbol">
                    {{ a.icon }} {{ a.name }} ({{ a.balance }} {{ a.symbol }})
                  </option>
                </select>
              </div>
              <div class="m-field">
                <label>Recipient Address</label>
                <input class="m-input" v-model="sendForm.address" placeholder="0x1a2b3c4d..." />
              </div>
              <div class="m-field">
                <label>Amount</label>
                <div class="amount-input-wrap">
                  <input class="m-input" type="number" v-model="sendForm.amount" placeholder="0.00" />
                  <span class="amount-sym">{{ sendForm.asset }}</span>
                </div>
                <p class="amount-hint" v-if="sendUSDValue">≈ ${{ formatUSD(sendUSDValue) }} USD</p>
              </div>
              <div class="m-field">
                <label>Network Fee</label>
                <div class="fee-options">
                  <div
                    v-for="fee in feeOptions"
                    :key="fee.label"
                    class="fee-opt"
                    :class="{ active: sendForm.fee === fee.label }"
                    @click="sendForm.fee = fee.label"
                  >
                    <p class="fee-label">{{ fee.label }}</p>
                    <p class="fee-val">{{ fee.val }}</p>
                    <p class="fee-time">{{ fee.time }}</p>
                  </div>
                </div>
              </div>
              <button class="btn-primary full" @click="executeSend">
                {{ sendSuccess ? '✓ Transaction Sent!' : 'Confirm Send' }}
              </button>
            </div>
          </div>

          <!-- RECEIVE MODAL -->
          <div v-if="activeModal === 'receive'">
            <div class="modal-header">
              <h3>Receive Crypto</h3>
              <button class="modal-close" @click="activeModal = null">✕</button>
            </div>
            <div class="modal-body receive-body">
              <div class="qr-code">
                <svg viewBox="0 0 100 100" class="qr-svg">
                  <!-- QR Code visual pattern -->
                  <rect width="100" height="100" fill="var(--gray-1)"/>
                  <g fill="var(--white)">
                    <!-- Top-left corner square -->
                    <rect x="10" y="10" width="22" height="22" rx="2"/>
                    <rect x="14" y="14" width="14" height="14" rx="1" fill="var(--gray-1)"/>
                    <rect x="17" y="17" width="8" height="8"/>
                    <!-- Top-right corner square -->
                    <rect x="68" y="10" width="22" height="22" rx="2"/>
                    <rect x="72" y="14" width="14" height="14" rx="1" fill="var(--gray-1)"/>
                    <rect x="75" y="17" width="8" height="8"/>
                    <!-- Bottom-left corner square -->
                    <rect x="10" y="68" width="22" height="22" rx="2"/>
                    <rect x="14" y="72" width="14" height="14" rx="1" fill="var(--gray-1)"/>
                    <rect x="17" y="75" width="8" height="8"/>
                    <!-- Data pattern dots -->
                    <rect x="40" y="10" width="4" height="4"/><rect x="48" y="10" width="4" height="4"/><rect x="56" y="10" width="4" height="4"/>
                    <rect x="40" y="18" width="4" height="4"/><rect x="56" y="18" width="4" height="4"/>
                    <rect x="44" y="22" width="4" height="4"/><rect x="52" y="22" width="4" height="4"/>
                    <rect x="10" y="40" width="4" height="4"/><rect x="18" y="40" width="4" height="4"/><rect x="26" y="40" width="4" height="4"/>
                    <rect x="10" y="48" width="4" height="4"/><rect x="22" y="48" width="4" height="4"/>
                    <rect x="10" y="56" width="4" height="4"/><rect x="18" y="56" width="4" height="4"/><rect x="26" y="56" width="4" height="4"/>
                    <rect x="40" y="40" width="4" height="4"/><rect x="48" y="40" width="8" height="4"/><rect x="60" y="40" width="8" height="4"/><rect x="72" y="40" width="4" height="4"/><rect x="80" y="40" width="10" height="4"/>
                    <rect x="40" y="48" width="8" height="4"/><rect x="52" y="48" width="4" height="4"/><rect x="60" y="48" width="4" height="4"/><rect x="68" y="48" width="8" height="4"/><rect x="80" y="48" width="4" height="4"/>
                    <rect x="40" y="56" width="4" height="4"/><rect x="48" y="56" width="4" height="4"/><rect x="56" y="56" width="8" height="4"/><rect x="68" y="56" width="4" height="4"/><rect x="76" y="56" width="8" height="4"/>
                    <rect x="40" y="64" width="4" height="4"/><rect x="52" y="64" width="8" height="4"/><rect x="64" y="64" width="4" height="4"/><rect x="72" y="64" width="4" height="4"/><rect x="80" y="64" width="10" height="4"/>
                    <rect x="40" y="72" width="8" height="4"/><rect x="52" y="72" width="4" height="4"/><rect x="60" y="72" width="8" height="4"/><rect x="72" y="72" width="4" height="4"/><rect x="80" y="72" width="4" height="4"/>
                    <rect x="40" y="80" width="4" height="4"/><rect x="48" y="80" width="8" height="4"/><rect x="60" y="80" width="4" height="4"/><rect x="68" y="80" width="4" height="4"/><rect x="76" y="80" width="8" height="4"/>
                    <rect x="40" y="88" width="4" height="4"/><rect x="52" y="88" width="4" height="4"/><rect x="60" y="88" width="8" height="4"/><rect x="72" y="88" width="4" height="4"/><rect x="80" y="88" width="10" height="4"/>
                    <!-- Acid green center logo mark -->
                    <rect x="44" y="44" width="12" height="12" rx="2" fill="#c8f53b"/>
                  </g>
                </svg>
              </div>
              <p class="receive-label">Your Wallet Address</p>
              <div class="address-box">
                <span class="address-text">0x3Fa4B7c2...9dE1A8f3</span>
                <button class="copy-btn" @click="copyAddress">{{ copied ? '✓' : '⧉' }}</button>
              </div>
              <div class="receive-assets">
                <p class="recv-note">Supports:</p>
                <div class="recv-chips">
                  <span v-for="a in walletAssets" :key="a.symbol" class="recv-chip" :style="{ borderColor: a.color, color: a.color }">
                    {{ a.icon }} {{ a.symbol }}
                  </span>
                </div>
              </div>
            </div>
          </div>

          <!-- BUY MODAL -->
          <div v-if="activeModal === 'buy'">
            <div class="modal-header">
              <h3>Buy {{ selectedAsset?.name }}</h3>
              <button class="modal-close" @click="activeModal = null">✕</button>
            </div>
            <div class="modal-body">
              <div class="buy-amounts">
                <button
                  v-for="amt in quickAmounts"
                  :key="amt"
                  class="quick-amt"
                  :class="{ active: buyAmount === amt }"
                  @click="buyAmount = amt"
                >${{ amt }}</button>
              </div>
              <div class="m-field">
                <label>Or enter custom amount (USD)</label>
                <input class="m-input" type="number" v-model="buyAmount" placeholder="0.00" />
              </div>
              <div class="buy-summary" v-if="buyAmount && selectedAsset">
                <div class="bs-row"><span>You Pay</span><span>${{ formatUSD(buyAmount) }}</span></div>
                <div class="bs-row"><span>You Get ≈</span><span>{{ (buyAmount / selectedAsset.price).toFixed(6) }} {{ selectedAsset.symbol }}</span></div>
                <div class="bs-row"><span>Network Fee</span><span>$2.50</span></div>
                <div class="bs-row total"><span>Total</span><span>${{ formatUSD(Number(buyAmount) + 2.5) }}</span></div>
              </div>
              <button class="btn-primary full" @click="executeBuy">
                {{ buySuccess ? '✓ Order Placed!' : `Buy ${selectedAsset?.symbol || 'Crypto'}` }}
              </button>
            </div>
          </div>

        </div>
      </div>
    </transition>

  </div>
</template>

<script>
export default {
  name: 'CryptoView',
  data() {
    return {
      assetView: 'list',
      txFilter: 'All',
      txFilters: ['All', 'Received', 'Sent', 'Swapped'],
      chartRange: '7D',
      chartRanges: ['1D', '7D', '1M', '3M', '1Y'],
      activeModal: null,
      copied: false,
      sendSuccess: false,
      buySuccess: false,
      buyAmount: 100,
      quickAmounts: [50, 100, 250, 500],
      selectedAsset: null,
      detailChartData: [],

      sendForm: {
        asset: 'BTC',
        address: '',
        amount: '',
        fee: 'Standard',
      },
      feeOptions: [
        { label: 'Slow', val: '~$0.80', time: '~10 min' },
        { label: 'Standard', val: '~$2.50', time: '~3 min' },
        { label: 'Fast', val: '~$6.00', time: '~30 sec' },
      ],

      walletAssets: [
        { name: 'Bitcoin', symbol: 'BTC', icon: '₿', balance: 0.4821, price: 67432, change24h: 2.34, color: '#f7931a', mktcap: '$1.32T', vol: '$28.4B', spark: [42,45,43,48,52,49,55,58,54,60,57,62] },
        { name: 'Ethereum', symbol: 'ETH', icon: 'Ξ', balance: 3.204, price: 3541, change24h: -1.18, color: '#627eea', mktcap: '$425B', vol: '$14.2B', spark: [60,58,62,55,52,56,53,50,54,51,48,52] },
        { name: 'Solana', symbol: 'SOL', icon: '◎', balance: 28.5, price: 182, change24h: 5.67, color: '#9945ff', mktcap: '$82B', vol: '$3.8B', spark: [30,35,32,40,38,45,42,50,48,55,52,58] },
        { name: 'USD Coin', symbol: 'USDC', icon: '◈', balance: 1240, price: 1.00, change24h: 0.01, color: '#2775ca', mktcap: '$34B', vol: '$6.1B', spark: [24,24,24,24,24,24,24,24,24,24,24,24] },
        { name: 'Chainlink', symbol: 'LINK', icon: '⬡', balance: 85, price: 18.42, change24h: 3.21, color: '#2a5ada', mktcap: '$11B', vol: '$892M', spark: [18,20,19,22,24,21,26,24,28,25,30,28] },
      ],

      transactions: [
        { id: 1, type: 'received', title: 'Received BTC', date: 'Mar 18, 2025', hash: '0x4f2a...', amount: '0.12', symbol: 'BTC', usd: 8091, status: 'confirmed' },
        { id: 2, type: 'sent', title: 'Sent ETH', date: 'Mar 17, 2025', hash: '0x8b1c...', amount: '0.5', symbol: 'ETH', usd: 1770, status: 'confirmed' },
        { id: 3, type: 'swapped', title: 'Swapped SOL → USDC', date: 'Mar 16, 2025', hash: '0x2e9d...', amount: '10', symbol: 'SOL', usd: 1820, status: 'confirmed' },
        { id: 4, type: 'received', title: 'Received USDC', date: 'Mar 15, 2025', hash: '0x7f3a...', amount: '500', symbol: 'USDC', usd: 500, status: 'confirmed' },
        { id: 5, type: 'sent', title: 'Sent LINK', date: 'Mar 14, 2025', hash: '0x1d5e...', amount: '20', symbol: 'LINK', usd: 368, status: 'confirmed' },
        { id: 6, type: 'received', title: 'Received SOL', date: 'Mar 13, 2025', hash: '0x9c2f...', amount: '5', symbol: 'SOL', usd: 910, status: 'pending' },
        { id: 7, type: 'sent', title: 'Sent BTC', date: 'Mar 12, 2025', hash: '0x3a8b...', amount: '0.05', symbol: 'BTC', usd: 3371, status: 'confirmed' },
      ],

      marketCoins: [
        { name: 'Bitcoin', symbol: 'BTC', icon: '₿', price: 67432, change: 2.34, color: '#f7931a' },
        { name: 'Ethereum', symbol: 'ETH', icon: 'Ξ', price: 3541, change: -1.18, color: '#627eea' },
        { name: 'BNB', symbol: 'BNB', icon: '◆', price: 612, change: 0.87, color: '#f3ba2f' },
        { name: 'Solana', symbol: 'SOL', icon: '◎', price: 182, change: 5.67, color: '#9945ff' },
        { name: 'XRP', symbol: 'XRP', icon: '◇', price: 0.612, change: -0.43, color: '#00aae4' },
        { name: 'Avalanche', symbol: 'AVAX', icon: '▲', price: 38.90, change: 4.12, color: '#e84142' },
      ],

      portfolioSparkData: [41200, 43500, 42100, 45800, 47200, 44900, 48600],
    }
  },

  computed: {
    totalValue() {
      return this.walletAssets.reduce((sum, a) => sum + a.balance * a.price, 0)
    },
    totalChange() {
      const weighted = this.walletAssets.reduce((sum, a) => {
        const val = a.balance * a.price
        return sum + (a.change24h * val)
      }, 0)
      return weighted / this.totalValue
    },
    chartLinePath() {
      return this.buildLinePath(this.portfolioSparkData, 200, 60, 3)
    },
    chartAreaPath() {
      return this.buildAreaPath(this.portfolioSparkData, 200, 60, 3)
    },
    detailLinePath() {
      return this.buildLinePath(this.detailChartData, 400, 120, 8)
    },
    detailAreaPath() {
      return this.buildAreaPath(this.detailChartData, 400, 120, 8)
    },
    detailEndX() {
      if (!this.detailChartData.length) return 400
      const n = this.detailChartData.length
      return 400 - 8
    },
    detailEndY() {
      if (!this.detailChartData.length) return 60
      const data = this.detailChartData
      const min = Math.min(...data), max = Math.max(...data)
      const last = data[data.length - 1]
      return 120 - 8 - ((last - min) / (max - min || 1)) * (120 - 16)
    },
    filteredTx() {
      if (this.txFilter === 'All') return this.transactions
      return this.transactions.filter(t => t.type === this.txFilter.toLowerCase())
    },
    donutSegments() {
      const total = this.walletAssets.reduce((s, a) => s + a.balance * a.price, 0)
      const circumference = 314
      let offset = 0
      return this.walletAssets.map(a => {
        const pct = (a.balance * a.price) / total
        const dash = pct * circumference
        const seg = { color: a.color, dash, offset }
        offset += dash
        return seg
      })
    },
    sendUSDValue() {
      if (!this.sendForm.amount) return 0
      const asset = this.walletAssets.find(a => a.symbol === this.sendForm.asset)
      return asset ? this.sendForm.amount * asset.price : 0
    },
  },

  methods: {
    formatUSD(val) {
      if (val >= 1000) return Number(val).toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })
      if (val >= 1) return Number(val).toFixed(2)
      return Number(val).toFixed(4)
    },
    sparkPoints(data) {
      const min = Math.min(...data), max = Math.max(...data)
      return data.map((v, i) => {
        const x = (i / (data.length - 1)) * 60
        const y = 24 - ((v - min) / (max - min || 1)) * 20 - 2
        return `${x},${y}`
      }).join(' ')
    },
    buildLinePath(data, w, h, pad) {
      if (!data.length) return ''
      const min = Math.min(...data), max = Math.max(...data)
      const pts = data.map((v, i) => {
        const x = pad + (i / (data.length - 1)) * (w - pad * 2)
        const y = h - pad - ((v - min) / (max - min || 1)) * (h - pad * 2)
        return `${x},${y}`
      })
      return 'M ' + pts.join(' L ')
    },
    buildAreaPath(data, w, h, pad) {
      const line = this.buildLinePath(data, w, h, pad)
      if (!line) return ''
      return `${line} L ${w - pad},${h - pad} L ${pad},${h - pad} Z`
    },
    selectAsset(asset) {
      this.selectedAsset = asset
      this.generateChartData()
    },
    generateChartData() {
      const points = { '1D': 24, '7D': 28, '1M': 30, '3M': 90, '1Y': 52 }[this.chartRange] || 28
      const base = this.selectedAsset?.price || 1000
      const volatility = 0.04
      let val = base * 0.88
      this.detailChartData = Array.from({ length: points }, () => {
        val = val * (1 + (Math.random() - 0.48) * volatility)
        return val
      })
      this.detailChartData[this.detailChartData.length - 1] = base
    },
    allocationPct(asset) {
      const total = this.walletAssets.reduce((s, a) => s + a.balance * a.price, 0)
      return ((asset.balance * asset.price / total) * 100).toFixed(1)
    },
    showModal(type) {
      this.activeModal = type
      this.sendSuccess = false
      this.buySuccess = false
    },
    copyAddress() {
      this.copied = true
      setTimeout(() => (this.copied = false), 2000)
    },
    executeSend() {
      if (!this.sendForm.address || !this.sendForm.amount) return
      this.sendSuccess = true
      setTimeout(() => { this.activeModal = null; this.sendSuccess = false }, 2500)
    },
    executeBuy() {
      this.buySuccess = true
      setTimeout(() => { this.activeModal = null; this.buySuccess = false }, 2500)
    },
  },

  mounted() {
    this.selectedAsset = this.walletAssets[0]
    this.generateChartData()
    // Simulate live price ticks
    this.priceTimer = setInterval(() => {
      this.walletAssets.forEach(a => {
        const tick = (Math.random() - 0.499) * 0.002
        a.price = parseFloat((a.price * (1 + tick)).toFixed(a.price >= 100 ? 2 : 4))
      })
      this.marketCoins.forEach(c => {
        const tick = (Math.random() - 0.499) * 0.003
        c.price = parseFloat((c.price * (1 + tick)).toFixed(c.price >= 100 ? 2 : 4))
        c.change = parseFloat((c.change + (Math.random() - 0.5) * 0.1).toFixed(2))
      })
    }, 2000)
  },

  beforeUnmount() {
    clearInterval(this.priceTimer)
  },
}
</script>

<style scoped>
.crypto-page { padding-top: 6rem; min-height: 100vh; }

/* Hero */
.page-hero {
  padding: 3rem 0 0;
  border-bottom: 1px solid var(--gray-3);
  margin-bottom: 0;
}
.container { max-width: 1280px; margin: 0 auto; padding: 0 2rem; }
.hero-top { display: flex; justify-content: space-between; align-items: flex-end; margin-bottom: 2rem; flex-wrap: wrap; gap: 1rem; }
.section-label { font-family: var(--font-mono); font-size: .72rem; color: var(--acid); text-transform: uppercase; letter-spacing: .15em; margin-bottom: .8rem; }
.page-title { font-family: var(--font-display); font-size: clamp(3rem, 7vw, 5.5rem); line-height: .95; }
.page-title span { color: var(--acid); }
.hero-actions { display: flex; gap: .75rem; }

.btn-primary {
  display: inline-flex; align-items: center; gap: .5rem;
  padding: .7rem 1.6rem; background: var(--acid); color: var(--black);
  font-weight: 500; font-size: .85rem; border-radius: 4px; border: none;
  cursor: pointer; font-family: var(--font-body); transition: all var(--transition);
}
.btn-primary:hover { background: var(--acid-dim); transform: translateY(-1px); }
.btn-primary.small { padding: .55rem 1.2rem; font-size: .8rem; }
.btn-primary.full { width: 100%; justify-content: center; padding: .9rem; font-size: .9rem; }
.btn-outline {
  display: inline-flex; align-items: center; gap: .5rem;
  padding: .7rem 1.6rem; background: transparent; color: var(--white);
  font-size: .85rem; border-radius: 4px; border: 1px solid var(--gray-3);
  cursor: pointer; font-family: var(--font-body); transition: all var(--transition);
}
.btn-outline:hover { border-color: var(--acid); color: var(--acid); }
.btn-outline.small { padding: .55rem 1.2rem; font-size: .8rem; }
.btn-icon { font-size: .9rem; }

/* Balance Card */
.balance-card {
  position: relative; overflow: hidden;
  background: var(--gray-1); border: 1px solid var(--gray-3);
  border-bottom: none; border-radius: 12px 12px 0 0;
  padding: 2rem 2.5rem;
}
.balance-bg {
  position: absolute; inset: 0;
  background: radial-gradient(ellipse 60% 80% at 80% 50%, rgba(200,245,59,.07), transparent);
  pointer-events: none;
}
.balance-inner { display: flex; justify-content: space-between; align-items: center; position: relative; z-index: 1; }
.balance-label { font-family: var(--font-mono); font-size: .7rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: .1em; margin-bottom: .5rem; }
.balance-amount { font-family: var(--font-display); font-size: clamp(2.5rem, 5vw, 4rem); line-height: 1; margin-bottom: .6rem; letter-spacing: .02em; }
.balance-change { display: inline-flex; align-items: center; gap: .4rem; font-family: var(--font-mono); font-size: .8rem; font-weight: 500; padding: .25rem .7rem; border-radius: 100px; }
.positive { color: var(--acid); background: rgba(200,245,59,.1); }
.negative { color: var(--red); background: rgba(255,59,59,.1); }
.change-arrow { font-size: .7rem; }
.change-period { color: var(--gray-5); font-size: .72rem; }
.mini-chart { width: 160px; height: 50px; }
.mini-chart svg { width: 100%; height: 100%; }
.chart-label { font-family: var(--font-mono); font-size: .62rem; color: var(--gray-5); text-align: right; margin-top: .3rem; }

/* Main Grid */
.main-grid {
  display: grid; grid-template-columns: 1fr 380px; gap: 1.5rem;
  padding-top: 1.5rem; padding-bottom: 4rem; align-items: start;
}

/* Panels */
.panel { background: var(--gray-1); border: 1px solid var(--gray-3); border-radius: 10px; overflow: hidden; margin-bottom: 1.5rem; }
.panel-header { display: flex; justify-content: space-between; align-items: center; padding: 1.2rem 1.5rem; border-bottom: 1px solid var(--gray-3); }
.panel-title { font-size: 1rem; font-weight: 500; }
.view-toggle { display: flex; background: var(--gray-2); border-radius: 6px; padding: 3px; gap: 2px; }
.view-toggle button { padding: .25rem .7rem; font-size: .72rem; font-family: var(--font-mono); color: var(--gray-5); background: transparent; border: none; cursor: pointer; border-radius: 4px; transition: all var(--transition); }
.view-toggle button.active { background: var(--gray-3); color: var(--white); }
.tx-filters { display: flex; gap: .3rem; }
.tx-filters button { padding: .2rem .65rem; font-size: .7rem; font-family: var(--font-mono); color: var(--gray-5); background: transparent; border: 1px solid transparent; border-radius: 100px; cursor: pointer; transition: all var(--transition); }
.tx-filters button:hover { color: var(--white); }
.tx-filters button.active { border-color: var(--acid); color: var(--acid); }

/* Asset List */
.asset-list { }
.asset-row {
  display: grid; grid-template-columns: 44px 1fr 120px 120px;
  align-items: center; gap: 1rem; padding: .9rem 1.5rem;
  border-bottom: 1px solid var(--gray-3); cursor: pointer;
  transition: background var(--transition);
}
.asset-row:last-child { border-bottom: none; }
.asset-row:hover, .asset-row.selected { background: var(--gray-2); }
.asset-icon { width: 38px; height: 38px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1rem; font-weight: 700; flex-shrink: 0; }
.asset-name { font-size: .9rem; font-weight: 500; }
.asset-symbol { font-size: .75rem; color: var(--gray-5); font-family: var(--font-mono); margin-top: .1rem; }
.asset-usd { font-size: .9rem; font-weight: 500; text-align: right; }
.asset-change { font-family: var(--font-mono); font-size: .72rem; text-align: right; margin-top: .1rem; }
.asset-price { font-family: var(--font-mono); font-size: .8rem; color: var(--gray-6); text-align: right; }
.asset-price-col { display: flex; flex-direction: column; align-items: flex-end; gap: .3rem; }
.spark { width: 60px; height: 24px; }
.spark svg { width: 100%; height: 100%; }

/* Asset Grid */
.asset-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: .75rem; padding: 1rem; }
.asset-card { padding: 1rem; background: var(--gray-2); border-radius: 8px; border: 1px solid var(--gray-3); cursor: pointer; transition: all var(--transition); }
.asset-card:hover, .asset-card.selected { border-color: var(--ac); }
.ac-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: .7rem; }
.ac-icon { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: .85rem; font-weight: 700; }
.ac-change { font-family: var(--font-mono); font-size: .68rem; }
.ac-name { font-size: .78rem; color: var(--gray-5); margin-bottom: .2rem; }
.ac-usd { font-size: .95rem; font-weight: 500; margin-bottom: .1rem; }
.ac-bal { font-family: var(--font-mono); font-size: .68rem; color: var(--gray-5); }

/* Transactions */
.tx-list { }
.tx-row { display: grid; grid-template-columns: 36px 1fr auto 80px; align-items: center; gap: 1rem; padding: .85rem 1.5rem; border-bottom: 1px solid var(--gray-3); }
.tx-row:last-child { border-bottom: none; }
.tx-icon { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: .85rem; flex-shrink: 0; }
.tx-icon.received { background: rgba(200,245,59,.12); color: var(--acid); }
.tx-icon.sent { background: rgba(255,59,59,.1); color: var(--red); }
.tx-icon.swapped { background: rgba(98,126,234,.15); color: #627eea; }
.tx-title { font-size: .85rem; font-weight: 400; }
.tx-date { font-family: var(--font-mono); font-size: .65rem; color: var(--gray-5); margin-top: .15rem; }
.tx-amount { text-align: right; }
.tx-amount p { font-family: var(--font-mono); font-size: .82rem; }
.tx-amount.received p { color: var(--acid); }
.tx-amount.sent p { color: var(--red); }
.tx-usd { font-size: .68rem; color: var(--gray-5); margin-top: .1rem; }
.tx-status { text-align: right; font-family: var(--font-mono); font-size: .65rem; text-transform: uppercase; letter-spacing: .06em; padding: .2rem .6rem; border-radius: 100px; }
.tx-status.confirmed { background: rgba(200,245,59,.1); color: var(--acid); }
.tx-status.pending { background: rgba(253,203,110,.1); color: #fdcb6e; }

/* Detail Panel */
.detail-panel { }
.detail-header { display: flex; align-items: center; gap: 1rem; padding: 1.2rem 1.5rem; border-bottom: 1px solid var(--gray-3); }
.detail-icon { width: 42px; height: 42px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.1rem; font-weight: 700; flex-shrink: 0; }
.detail-name { font-size: 1rem; font-weight: 500; }
.detail-sym { font-family: var(--font-mono); font-size: .7rem; color: var(--gray-5); }
.detail-price-block { margin-left: auto; text-align: right; }
.detail-price { font-family: var(--font-display); font-size: 1.4rem; line-height: 1; }
.detail-change { font-family: var(--font-mono); font-size: .72rem; margin-top: .3rem; display: block; padding: .15rem .5rem; border-radius: 100px; }
.detail-stats { display: grid; grid-template-columns: 1fr 1fr; gap: 1px; background: var(--gray-3); border-bottom: 1px solid var(--gray-3); }
.d-stat { padding: .9rem 1.2rem; background: var(--gray-1); }
.ds-label { font-family: var(--font-mono); font-size: .62rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: .08em; margin-bottom: .3rem; }
.ds-val { font-size: .9rem; font-weight: 500; }
.detail-chart { padding: 1rem 1.2rem; }
.chart-tabs { display: flex; gap: .3rem; margin-bottom: .8rem; }
.chart-tabs button { padding: .2rem .6rem; font-size: .68rem; font-family: var(--font-mono); color: var(--gray-5); background: transparent; border: 1px solid transparent; border-radius: 3px; cursor: pointer; transition: all var(--transition); }
.chart-tabs button.active { border-color: var(--acid); color: var(--acid); }
.line-chart { width: 100%; height: 100px; display: block; }
.detail-actions { display: grid; grid-template-columns: 1fr 1fr; gap: .75rem; padding: 1rem 1.5rem; border-top: 1px solid var(--gray-3); }

/* Allocation Donut */
.allocation-chart { padding: 1.5rem; display: flex; align-items: center; gap: 2rem; }
.donut-svg { width: 120px; height: 120px; flex-shrink: 0; }
.donut-legend { display: flex; flex-direction: column; gap: .6rem; flex: 1; }
.legend-item { display: flex; align-items: center; gap: .5rem; font-size: .8rem; }
.legend-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
.legend-name { color: var(--gray-6); flex: 1; font-family: var(--font-mono); font-size: .72rem; }
.legend-pct { font-family: var(--font-mono); font-size: .72rem; color: var(--white); }

/* Market Watch */
.live-badge { display: flex; align-items: center; gap: .4rem; font-family: var(--font-mono); font-size: .65rem; color: var(--acid); }
.live-dot { width: 5px; height: 5px; border-radius: 50%; background: var(--acid); animation: pulse 1.5s infinite; }
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.2} }
.market-list { }
.market-row { display: flex; align-items: center; gap: .8rem; padding: .8rem 1.2rem; border-bottom: 1px solid var(--gray-3); }
.market-row:last-child { border-bottom: none; }
.mkt-icon { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: .85rem; font-weight: 700; flex-shrink: 0; }
.mkt-info { flex: 1; }
.mkt-name { font-size: .82rem; font-weight: 400; }
.mkt-sym { font-family: var(--font-mono); font-size: .65rem; color: var(--gray-5); }
.mkt-price-block { text-align: right; }
.mkt-price { font-family: var(--font-mono); font-size: .82rem; }
.mkt-change { font-family: var(--font-mono); font-size: .68rem; margin-top: .15rem; }

/* Modal */
.modal-overlay {
  position: fixed; inset: 0; background: rgba(0,0,0,.7); backdrop-filter: blur(8px);
  z-index: 2000; display: flex; align-items: center; justify-content: center; padding: 1rem;
}
.modal-box {
  background: var(--gray-1); border: 1px solid var(--gray-3); border-radius: 12px;
  width: 100%; max-width: 460px; max-height: 90vh; overflow-y: auto;
}
.modal-header { display: flex; justify-content: space-between; align-items: center; padding: 1.3rem 1.5rem; border-bottom: 1px solid var(--gray-3); }
.modal-header h3 { font-size: 1rem; font-weight: 500; }
.modal-close { width: 28px; height: 28px; border: 1px solid var(--gray-3); border-radius: 50%; background: none; color: var(--gray-5); cursor: pointer; font-size: .8rem; display: flex; align-items: center; justify-content: center; transition: all var(--transition); }
.modal-close:hover { border-color: var(--white); color: var(--white); }
.modal-body { padding: 1.5rem; display: flex; flex-direction: column; gap: 1.2rem; }
.m-field label { display: block; font-family: var(--font-mono); font-size: .65rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: .1em; margin-bottom: .5rem; }
.m-input { width: 100%; background: var(--gray-2); border: 1px solid var(--gray-3); border-radius: 4px; padding: .7rem .9rem; color: var(--white); font-family: var(--font-body); font-size: .88rem; outline: none; transition: border-color var(--transition); appearance: none; }
.m-input:focus { border-color: var(--acid); }
.m-input option { background: var(--gray-2); }
.amount-input-wrap { position: relative; }
.amount-sym { position: absolute; right: .9rem; top: 50%; transform: translateY(-50%); font-family: var(--font-mono); font-size: .75rem; color: var(--gray-5); }
.amount-hint { font-family: var(--font-mono); font-size: .72rem; color: var(--gray-5); margin-top: .4rem; }
.fee-options { display: grid; grid-template-columns: repeat(3,1fr); gap: .5rem; }
.fee-opt { padding: .7rem; background: var(--gray-2); border: 1px solid var(--gray-3); border-radius: 6px; cursor: pointer; text-align: center; transition: all var(--transition); }
.fee-opt:hover { border-color: var(--gray-4); }
.fee-opt.active { border-color: var(--acid); }
.fee-label { font-size: .78rem; font-weight: 500; margin-bottom: .2rem; }
.fee-val { font-family: var(--font-mono); font-size: .7rem; color: var(--acid); }
.fee-time { font-family: var(--font-mono); font-size: .62rem; color: var(--gray-5); margin-top: .1rem; }

/* Receive modal */
.receive-body { align-items: center; text-align: center; }
.qr-code { background: var(--gray-2); border: 1px solid var(--gray-3); border-radius: 10px; padding: 1rem; display: inline-block; }
.qr-svg { width: 160px; height: 160px; display: block; }
.receive-label { font-family: var(--font-mono); font-size: .68rem; color: var(--gray-5); text-transform: uppercase; letter-spacing: .1em; }
.address-box { display: flex; align-items: center; gap: .5rem; background: var(--gray-2); border: 1px solid var(--gray-3); border-radius: 6px; padding: .6rem 1rem; width: 100%; }
.address-text { font-family: var(--font-mono); font-size: .78rem; color: var(--gray-6); flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.copy-btn { padding: .25rem .6rem; background: var(--gray-3); border: none; color: var(--white); border-radius: 4px; cursor: pointer; font-size: .8rem; flex-shrink: 0; }
.receive-assets { width: 100%; }
.recv-note { font-family: var(--font-mono); font-size: .65rem; color: var(--gray-5); margin-bottom: .5rem; }
.recv-chips { display: flex; flex-wrap: wrap; gap: .4rem; justify-content: center; }
.recv-chip { padding: .2rem .7rem; border-radius: 100px; border: 1px solid; font-family: var(--font-mono); font-size: .68rem; }

/* Buy modal */
.buy-amounts { display: grid; grid-template-columns: repeat(4,1fr); gap: .5rem; }
.quick-amt { padding: .55rem; background: var(--gray-2); border: 1px solid var(--gray-3); border-radius: 6px; color: var(--gray-6); font-size: .82rem; cursor: pointer; font-family: var(--font-body); transition: all var(--transition); }
.quick-amt:hover { border-color: var(--gray-4); }
.quick-amt.active { border-color: var(--acid); color: var(--acid); }
.buy-summary { background: var(--gray-2); border-radius: 8px; padding: 1rem; display: flex; flex-direction: column; gap: .5rem; }
.bs-row { display: flex; justify-content: space-between; font-size: .85rem; color: var(--gray-6); }
.bs-row span:last-child { font-family: var(--font-mono); }
.bs-row.total { padding-top: .5rem; border-top: 1px solid var(--gray-3); color: var(--white); font-weight: 500; margin-top: .2rem; }

/* Modal transition */
.modal-enter-active, .modal-leave-active { transition: opacity .25s ease, transform .25s ease; }
.modal-enter-from, .modal-leave-to { opacity: 0; transform: scale(.96) translateY(10px); }

/* Responsive */
@media (max-width: 1024px) { .main-grid { grid-template-columns: 1fr; } }
@media (max-width: 768px) {
  .balance-inner { flex-direction: column; align-items: flex-start; gap: 1rem; }
  .asset-row { grid-template-columns: 44px 1fr auto; }
  .asset-price-col { display: none; }
  .tx-row { grid-template-columns: 36px 1fr auto; }
  .tx-status { display: none; }
  .asset-grid { grid-template-columns: repeat(2,1fr); }
}
</style>