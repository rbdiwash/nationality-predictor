<template>
  <div
    class="min-h-screen bg-gradient-to-br from-slate-50 via-indigo-50/30 to-purple-50"
  >
    <!-- Header Section -->
    <div
      class="bg-gradient-to-r from-indigo-600 via-purple-600 to-indigo-700 text-white shadow-xl"
    >
      <div class="max-w-7xl mx-auto px-6 py-8 lg:py-12">
        <div
          class="flex flex-col lg:flex-row items-start lg:items-center justify-between gap-6"
        >
          <div class="flex-1">
            <div class="flex items-center gap-3 mb-3">
              <span
                class="text-sm uppercase tracking-wider text-indigo-200 font-semibold"
              >
                AI Powered
              </span>
              <span
                class="px-3 py-1 text-xs font-semibold bg-white/20 backdrop-blur-sm rounded-full"
              >
                Live
              </span>
            </div>
            <h1
              class="flex items-center gap-2 text-4xl lg:text-6xl font-bold leading-tight mb-4"
            >
              <img
                src="../assets/logo_generator.png"
                alt="Nationality Predictor"
                class="w-10 h-10"
              />
              Nationality Predictor
            </h1>
            <p class="text-lg lg:text-xl text-indigo-100 max-w-2xl">
              Predict the most likely nationalities from any name with
              AI-powered insights
            </p>
          </div>

          <div class="w-full lg:w-auto lg:min-w-[400px]">
            <form @submit.prevent="handleSubmit" class="space-y-4">
              <div>
                <label
                  for="name-input"
                  class="block text-sm font-semibold text-white mb-2"
                >
                  Enter a Name
                </label>
                <div class="relative">
                  <input
                    id="name-input"
                    v-model="name"
                    type="text"
                    placeholder="e.g., John, Maria, Ahmed"
                    class="w-full px-5 py-4 text-base bg-white/95 backdrop-blur-sm border-2 border-white/20 rounded-xl transition-all duration-300 focus:outline-none focus:border-white focus:ring-4 focus:ring-white/30 text-slate-900 placeholder:text-slate-400"
                    required
                    @change="handleNameChange"
                  />
                </div>
              </div>
              <button
                type="submit"
                class="w-full py-4 text-lg font-semibold text-indigo-600 bg-white rounded-xl border-none cursor-pointer transition-all duration-300 flex items-center justify-center gap-2 hover:scale-[1.02] hover:shadow-2xl disabled:opacity-70 disabled:cursor-not-allowed disabled:hover:scale-100"
                :disabled="loading"
              >
                <span v-if="!loading || showInsight"
                  >🔮 Predict Nationality</span
                >
                <span v-else class="flex items-center gap-2">
                  <span
                    class="w-5 h-5 border-2 border-indigo-600/30 border-t-indigo-600 rounded-full animate-spin"
                  ></span>
                  Predicting...
                </span>
              </button>
              <button
                @click="handleSubmitWithInsight"
                class="w-full py-4 text-lg font-semibold text-indigo-600 bg-white rounded-xl border-none cursor-pointer transition-all duration-300 flex items-center justify-center gap-2 hover:scale-[1.02] hover:shadow-2xl disabled:opacity-70 disabled:cursor-not-allowed disabled:hover:scale-100"
                :disabled="loading"
              >
                <span v-if="!loading || !showInsight"
                  >🔮 Predict Nationality with Insight</span
                >
                <span v-else class="flex items-center gap-2">
                  <span
                    class="w-5 h-5 border-2 border-indigo-600/30 border-t-indigo-600 rounded-full animate-spin"
                  ></span>
                  Predicting with Insight...
                </span>
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Main Content Area -->
    <div class="max-w-7xl mx-auto px-6 py-8">
      <!-- Error Message -->
      <div
        v-if="error"
        class="mb-6 p-4 bg-red-50 border-2 border-red-200 rounded-xl text-red-700 flex items-center gap-3 shadow-sm"
      >
        <span class="text-2xl">⚠️</span>
        <span class="font-medium">{{ error }}</span>
      </div>

      <!-- No Results Message -->
      <div
        v-if="apiHit && results && results.length === 0 && name"
        class="mb-8 p-8 bg-gradient-to-r from-blue-50 to-indigo-50 border-2 border-blue-200 rounded-2xl shadow-lg animate-slide-up"
      >
        <div class="text-center">
          <div class="text-6xl mb-4">🔍</div>
          <h3 class="text-2xl font-bold text-slate-900 mb-2">
            No Results Found
          </h3>
          <p class="text-lg text-slate-700 mb-4">
            We couldn't find any nationality predictions for
            <span class="font-semibold text-indigo-600">"{{ name }}"</span>
          </p>
          <p class="text-sm text-slate-600">
            Try entering a different name or check the spelling.
          </p>
        </div>
      </div>
    
      <!-- Insight Section -->
      <div
        v-if="insight && !loading && showInsight"
        class="mb-8 p-6 bg-gradient-to-r from-amber-50 to-orange-50 border-2 border-amber-200 rounded-2xl shadow-lg animate-slide-up"
      >
        <h2
          class="text-2xl font-bold text-slate-900 mb-3 flex items-center gap-3"
        >
          <span class="text-3xl">💡</span>
          <span>AI Insight</span>
        </h2>
        <p
          class="text-base lg:text-lg text-slate-700 leading-relaxed"
          v-html="insight"
        ></p>
      </div>

      <!-- Results Section with Horizontal Scroll -->
      <div v-if="results && results.length > 0" class="animate-slide-up">
        <div class="flex items-center justify-between mb-6">
          <div>
            <h2 class="text-3xl font-bold text-slate-900 mb-2">
              Prediction Results
            </h2>
            <p class="text-slate-600">
              Found {{ results.length }}
              {{ results.length === 1 ? "nationality" : "nationalities" }}
            </p>
          </div>
          <div class="hidden lg:flex items-center gap-2 text-sm text-slate-500">
            <span>← Scroll →</span>
          </div>
        </div>

        <!-- Horizontal Scrollable Container -->
        <div
          class="overflow-x-auto pb-6 -mx-6 px-6"
          style="scrollbar-width: thin; scrollbar-color: #cbd5e1 #f1f5f9"
        >
          <div class="flex gap-6 min-w-max">
            <div
              v-for="(result, index) in results"
              :key="result.countryCode"
              class="flex-shrink-0 w-80 lg:w-96 p-6 rounded-2xl transition-all duration-300 hover:scale-105 hover:shadow-2xl border-2"
              :class="
                index === 0
                  ? 'bg-gradient-to-br from-amber-50 via-orange-50 to-amber-100 border-amber-400 shadow-xl'
                  : 'bg-white border-slate-200 hover:border-indigo-300 shadow-lg'
              "
            >
              <!-- Country Header -->
              <div class="flex items-start justify-between mb-6">
                <div class="flex items-center gap-4">
                  <div class="text-5xl leading-none">
                    {{ getCountryFlag(result.countryCode) }}
                  </div>
                  <div>
                    <h3 class="text-xl font-bold text-slate-900 mb-1">
                      {{ getCountryName(result.countryCode) }}
                    </h3>
                    <p
                      class="text-sm font-semibold text-slate-500 uppercase tracking-wider"
                    >
                      {{ result.countryCode }}
                    </p>
                  </div>
                </div>
                <span
                  v-if="index === 0"
                  class="px-3 py-1.5 text-xs font-bold text-amber-700 bg-white rounded-full shadow-sm border border-amber-300"
                >
                  ⭐ Most Likely
                </span>
              </div>

              <!-- Pie Chart -->
              <div class="flex flex-col items-center gap-6 mb-6">
                <div
                  class="relative w-32 h-32 flex items-center justify-center rounded-full border-4 border-slate-200 shadow-inner"
                  :style="pieStyle(result.probability, index === 0)"
                  aria-hidden="true"
                >
                  <div
                    class="w-24 h-24 rounded-full bg-white flex items-center justify-center shadow-lg border-2 border-slate-100"
                  >
                    <div class="text-center">
                      <div
                        class="text-3xl font-bold"
                        :class="
                          index === 0 ? 'text-amber-600' : 'text-indigo-600'
                        "
                      >
                        {{ (result.probability * 100).toFixed(0) }}%
                      </div>
                      <div class="text-xs text-slate-500 font-medium">
                        probability
                      </div>
                    </div>
                  </div>
                </div>

                <!-- Progress Bar -->
                <div class="w-full">
                  <div class="flex items-center justify-between mb-2">
                    <span class="text-sm font-semibold text-slate-700"
                      >Probability</span
                    >
                    <span class="text-sm font-bold text-slate-900">
                      {{ (result.probability * 100).toFixed(1) }}%
                    </span>
                  </div>
                  <div
                    class="w-full h-3 bg-slate-200 rounded-full overflow-hidden shadow-inner"
                  >
                    <div
                      class="h-full rounded-full transition-all duration-700 ease-out"
                      :class="
                        index === 0
                          ? 'bg-gradient-to-r from-amber-500 to-orange-500'
                          : 'bg-gradient-to-r from-indigo-500 to-purple-600'
                      "
                      :style="{ width: `${result.probability * 100}%` }"
                    ></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div
        v-if="!results && !loading && !error && !name"
        class="text-center py-16 lg:py-24"
      >
        <div class="max-w-md mx-auto">
          <div class="text-6xl mb-4">🌍</div>
          <h3 class="text-2xl font-bold text-slate-900 mb-3">
            Ready to Predict?
          </h3>
          <p class="text-lg text-slate-600 mb-6">
            Enter a name above to discover the most likely nationalities with
            AI-powered predictions
          </p>
          <div
            class="flex flex-wrap justify-center gap-4 text-sm text-slate-500"
          >
            <span class="flex items-center gap-2">
              <span>⚡</span> Instant Results
            </span>
            <span class="flex items-center gap-2">
              <span>📊</span> Visual Charts
            </span>
            <span class="flex items-center gap-2">
              <span>💡</span> AI Insights
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const name = ref("");
const loading = ref(false);
const error = ref("");
const results = ref<Array<{ countryCode: string; probability: number }>>([]);
const config = useRuntimeConfig();
const backendUrl = config?.public?.backendUrl;
const insight = ref<string>("");
const showInsight = ref(false);
const apiHit = ref(false);

const pieStyle = (probability: number, isTop: boolean) => {
  const percent = Math.max(0, Math.min(probability * 100, 100));
  const primary = isTop ? "#f59e0b" : "#6366f1";
  const secondary = "#e5e7eb";
  return {
    background: `conic-gradient(${primary} 0% ${percent}%, ${secondary} ${percent}% 100%)`,
  };
};

const handleNameChange = () => {
apiHit.value = false;
  handleSubmit();
};

const handleSubmit = async () => {
  if (!name.value.trim()) return;
  showInsight.value = false;
  loading.value = true;
  error.value = "";
  results.value = [];

  try {
    const response = await fetch(`${backendUrl}/nationality/predict`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        name: name.value,
      }),
    });

    if (!response.ok) {
      throw new Error("Failed to fetch prediction");
    }
    const data = await response.json();
    results.value = data.countries || [];
    apiHit.value = true;
  } catch (err) {
    console.error("Error:", err);
    error.value = "Something went wrong. Please try again.";
  } finally {
    loading.value = false;
  }
};

const handleSubmitWithInsight = async () => {
  if (!name.value.trim()) return;
  showInsight.value = true;
  loading.value = true;
  error.value = "";
  results.value = [];

  try {
    const response = await fetch(
      `${backendUrl}/nationality/predict-with-insight`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name: name.value,
        }),
      }
    );

    if (!response.ok) {
      throw new Error("Failed to fetch prediction");
    }
    const data = await response.json();
    results.value = data.countries || [];
    insight.value = data.insight || "";
    apiHit.value = true;
  } catch (err) {
    console.error("Error:", err);
    error.value = "Something went wrong. Please try again.";
  } finally {
    loading.value = false;
  }
};

const getCountryFlag = (countryId: string): string => {
  // Convert country code to flag emoji
  const codePoints = countryId
    .toUpperCase()
    .split("")
    .map((char) => 127397 + char.charCodeAt(0));
  return String.fromCodePoint(...codePoints);
};

const getCountryName = (countryId: string): string => {
  const countryNames: Record<string, string> = {
    // North America
    US: "United States",
    CA: "Canada",
    MX: "Mexico",
    GT: "Guatemala",
    CU: "Cuba",
    HT: "Haiti",
    DO: "Dominican Republic",
    JM: "Jamaica",
    CR: "Costa Rica",
    PA: "Panama",
    HN: "Honduras",
    NI: "Nicaragua",
    SV: "El Salvador",
    BZ: "Belize",
    // South America
    BR: "Brazil",
    AR: "Argentina",
    CO: "Colombia",
    PE: "Peru",
    VE: "Venezuela",
    CL: "Chile",
    EC: "Ecuador",
    BO: "Bolivia",
    PY: "Paraguay",
    UY: "Uruguay",
    GY: "Guyana",
    SR: "Suriname",
    // Europe
    GB: "United Kingdom",
    DE: "Germany",
    FR: "France",
    IT: "Italy",
    ES: "Spain",
    PL: "Poland",
    NL: "Netherlands",
    BE: "Belgium",
    GR: "Greece",
    PT: "Portugal",
    CZ: "Czech Republic",
    RO: "Romania",
    HU: "Hungary",
    SE: "Sweden",
    AT: "Austria",
    CH: "Switzerland",
    BG: "Bulgaria",
    RS: "Serbia",
    DK: "Denmark",
    FI: "Finland",
    SK: "Slovakia",
    NO: "Norway",
    IE: "Ireland",
    HR: "Croatia",
    BA: "Bosnia and Herzegovina",
    AL: "Albania",
    LT: "Lithuania",
    SI: "Slovenia",
    LV: "Latvia",
    EE: "Estonia",
    IS: "Iceland",
    LU: "Luxembourg",
    MT: "Malta",
    CY: "Cyprus",
    UA: "Ukraine",
    BY: "Belarus",
    MD: "Moldova",
    // Asia
    CN: "China",
    IN: "India",
    JP: "Japan",
    KR: "South Korea",
    ID: "Indonesia",
    TH: "Thailand",
    VN: "Vietnam",
    PH: "Philippines",
    MY: "Malaysia",
    SG: "Singapore",
    PK: "Pakistan",
    BD: "Bangladesh",
    IR: "Iran",
    TR: "Turkey",
    SA: "Saudi Arabia",
    IQ: "Iraq",
    AF: "Afghanistan",
    NP: "Nepal",
    LK: "Sri Lanka",
    MM: "Myanmar",
    KH: "Cambodia",
    LA: "Laos",
    KZ: "Kazakhstan",
    UZ: "Uzbekistan",
    MN: "Mongolia",
    TW: "Taiwan",
    HK: "Hong Kong",
    AE: "United Arab Emirates",
    IL: "Israel",
    JO: "Jordan",
    LB: "Lebanon",
    SY: "Syria",
    YE: "Yemen",
    OM: "Oman",
    KW: "Kuwait",
    QA: "Qatar",
    BH: "Bahrain",
    AM: "Armenia",
    AZ: "Azerbaijan",
    GE: "Georgia",
    // Africa
    NG: "Nigeria",
    ET: "Ethiopia",
    EG: "Egypt",
    ZA: "South Africa",
    KE: "Kenya",
    UG: "Uganda",
    TZ: "Tanzania",
    GH: "Ghana",
    DZ: "Algeria",
    SD: "Sudan",
    MA: "Morocco",
    AO: "Angola",
    MZ: "Mozambique",
    MG: "Madagascar",
    CM: "Cameroon",
    CI: "Ivory Coast",
    NE: "Niger",
    BF: "Burkina Faso",
    ML: "Mali",
    MW: "Malawi",
    ZM: "Zambia",
    SN: "Senegal",
    TD: "Chad",
    SO: "Somalia",
    ZW: "Zimbabwe",
    GN: "Guinea",
    RW: "Rwanda",
    BJ: "Benin",
    TN: "Tunisia",
    BI: "Burundi",
    SS: "South Sudan",
    TG: "Togo",
    SL: "Sierra Leone",
    LY: "Libya",
    LR: "Liberia",
    MR: "Mauritania",
    ER: "Eritrea",
    GM: "Gambia",
    BW: "Botswana",
    GA: "Gabon",
    LS: "Lesotho",
    GW: "Guinea-Bissau",
    MU: "Mauritius",
    // Oceania
    AU: "Australia",
    NZ: "New Zealand",
    PG: "Papua New Guinea",
    FJ: "Fiji",
    SB: "Solomon Islands",
    NC: "New Caledonia",
    VU: "Vanuatu",
    // Russia & Central Asia
    RU: "Russia",
    TJ: "Tajikistan",
    KG: "Kyrgyzstan",
    TM: "Turkmenistan",
  };
  return countryNames[countryId] || countryId;
};
</script>

<style scoped>
/* Custom scrollbar for horizontal scroll */
.overflow-x-auto::-webkit-scrollbar {
  height: 8px;
}

.overflow-x-auto::-webkit-scrollbar-track {
  background: #f1f5f9;
  border-radius: 10px;
}

.overflow-x-auto::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 10px;
}

.overflow-x-auto::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>
