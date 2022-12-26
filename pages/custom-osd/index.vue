<script setup lang="ts">
const route = useRoute();
const defaultManifest: string = route.query.manifest
  ? String(route.query.manifest)
  : "https://dl.ndl.go.jp/api/iiif/3437686/manifest.json";

const page = ref<number>(1);
const regions = ref<string[]>([]);
const manifest = ref<string>(defaultManifest);

const move = (value: number) => {
  let currentPage = page.value;
  let nextPage = currentPage + value;
  if (nextPage < 1) {
    return;
  }
  page.value += value;
};

const updatePage = (value: number) => {
  page.value = value;
};

const ex01 = () => {
  regions.value = [];
  manifest.value = "https://www.dl.ndl.go.jp/api/iiif/3437686/manifest.json";
  page.value = 1;
};

const ex02 = () => {
  regions.value = [
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/69#xywh=2028,644,1452,1992",
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/69#xywh=544,620,1452,2000",
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/91#xywh=470,686,1548,1991",
  ];
  manifest.value = "https://www.dl.ndl.go.jp/api/iiif/2567061/manifest.json";
  page.value = 69;
};

const ex03 = () => {
  regions.value = [
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/69#xywh=2028,644,1452,1992",
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/69#xywh=544,620,1452,2000",
    "https://dl.ndl.go.jp/api/iiif/2567061/canvas/91#xywh=470,686,1548,1991",
    // "https://dl.ndl.go.jp/api/iiif/2567061/canvas/91#xywh=470,686,1548,1991"
  ];
  manifest.value = "https://www.dl.ndl.go.jp/api/iiif/2567061/manifest.json";
  page.value = 91;
};
</script>
<template>
  <v-container>
    <v-row>
      <v-col>
        <OsdCustomViewer
          :page="page"
          @updated="updatePage"
          :manifest="manifest"
          :regions="regions"
        />
      </v-col>
      <v-col>
        <div class="mb-5">
          <h3 class="highlight">現在のページ</h3>

          {{ page }}
        </div>

        <div class="mb-5">
          <h3>コンポーネントの外からページネーション</h3>

          <v-btn @click="move(1)" class="ma-1">+</v-btn>
          <v-btn @click="move(-1)" class="ma-1">-</v-btn>
        </div>

        <div class="mb-5">
          <h3>サンプル</h3>

          <v-btn class="ma-1" @click="ex01"> デフォルト（校異源氏物語） </v-btn>
          <v-btn class="ma-1" @click="ex02">
            画像の一部をハイライト（絵入源氏物語）
          </v-btn>
          <v-btn class="ma-1" @click="ex03">
            他のページをハイライト（絵入源氏物語）
          </v-btn>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>
