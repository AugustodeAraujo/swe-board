<template>
  <div class="bg-black min-h-screen">
    <Container>
      <h1 class="text-4xl font-bold">Battle Report</h1>
      <NuxtLink
        to="/battleboard"
        class="font-sm text-red-400 p-2 font-mono mb-2"
        >Voltar</NuxtLink
      >

      <div class="my-4">
        <div class="bg-zinc-900 p-4 border-l-4 rounded-lg border-red-400">
          <div class="flex flex-col">
            <p class="text-sm font-bold text-red-400">DAMAGE MVP</p>

            <div class="my-4">
              <p class="text-6xl text-white font-mono">
                {{ highestDamagePlayer.name }}
              </p>

              <p class="text-zinc-400 font-mono -mt-2">
                {{ highestDamagePlayer.guildName }}
              </p>
            </div>

            <div class="mt-4">
              <p class="text-white font-mono">
                Total damage:
                <span class="text-red-400">{{
                  highestDamagePlayer.totalDamage
                }}</span>
              </p>

              <div class="flex gap-8">
                <p class="text-white font-mono">
                  K/D {{ highestDamagePlayer.kills }}:{{ highestDamagePlayer.deaths }}
                </p>
              </div>

              <p class="text-white font-mono">
                Fame: {{ formatNumber(highestDamagePlayer.killFame) }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </Container>
  </div>
</template>

<script setup>
const route = useRoute();
const battleEndpoint = `https://api-east.albionbattles.com/battles/${route.params.id}`;
const { data } = await useFetch(battleEndpoint);
const battleInfo = data._rawValue;

const highestDamagePlayer = battleInfo.highestDamagePlayer.players[0];


function formatNumber(numero) {
  if (numero >= 1000000) {
    const milhoes = numero / 1000000;
    const milhoesFormatado = milhoes.toFixed(1);
    return milhoesFormatado.replace(".0", "") + "m";
  } else if (numero >= 1000) {
    const milhares = numero / 1000;
    const milharesFormatado = milhares.toFixed(1);
    return milharesFormatado.replace(".0", "") + "k";
  } else {
    return numero.toString();
  }
}
</script>

<style  scoped>
</style>