<script setup lang="ts">
const { $OpenSeadragon } = useNuxtApp();

const route = useRoute();

const manifest: string = route.query.manifest
  ? String(route.query.manifest)
  : "https://dl.ndl.go.jp/api/iiif/3437686/manifest.json";

const { data: results_ }: any = await useFetch(manifest);

const canvases = results_.value.sequences[0].canvases;

const tileSources: any[] = [];

for (const canvas of canvases) {
  const image = canvas.images[0].resource.service["@id"] + "/info.json";
  tileSources.push(image);
}

onMounted(() => {

  $OpenSeadragon({
    id: "viewer-image",
    prefixUrl: "./images/",
    tileSources,
  });
});

</script>
<template>
  <div style="background-color: black; height: 600px">
    <div id="viewer-image" ref="image" style="height: 100%; width: 100%" />
  </div>
</template>
