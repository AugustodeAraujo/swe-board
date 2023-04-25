<template>
  <div class="bg-black min-h-screen">
    <Container>
      <img
        src="@/assets/images/swelogo.png"
        alt="Sex With Ex Logo"
        loading="lazy"
        class="mx-auto mb-10"
      />

      <div class="rounded-lg border border-zinc-600 bg-zinc-900">
        <table class="table-auto w-full text-sm">
          <thead class="ltr:text-left rtl:text-right">
            <tr>
              <th class="text-left px-4 py-2 text-zinc-400">ID</th>
              <th class="text-left px-4 py-2 text-zinc-400">Alianças</th>

              <th class="text-right px-4 py-2 text-zinc-400">Players</th>
              <th class="px-4 py-2 text-zinc-400 text-right">
                Total de Abates
              </th>
              <th class="text-left px-4 py-2 text-zinc-400">Fama</th>
              <th class="text-left px-4 py-2 text-zinc-400">Start Time</th>
              <th class="text-left px-4 py-2 text-zinc-400">End Time</th>
            </tr>
          </thead>

          <tbody>
            <tr
              v-for="(battle, index) in battles"
              :key="index"
              class="bg-zinc-800 border-b border-zinc-700"
            >
              <td class="px-4 py-2 text-zinc-200 text-[10px] font-mono">
                <nuxt-link :to="`battles/${battle.id}`">
                  {{ battle.id }}
                </nuxt-link>
              </td>
              <td class="px-4 py-2 text-zinc-200">
                {{ battle.alliances.list[0] }}
              </td>

              <td class="px-4 py-2 text-zinc-200 text-right">
                {{ battle.players.list.length }}
              </td>
              <td
                class="px-4 py-2 text-zinc-200 text-right"
                :class="battle.totalKills > 20 ? 'text-red-500' : ''"
              >
                {{ battle.totalKills }}
              </td>
              <td class="px-4 py-2 text-zinc-200 font-mono">
                <span
                  :class="
                    battle.totalFame >= 500000
                      ? battle.totalFame >= 1000000
                        ? 'text-green-500'
                        : 'text-yellow-300'
                      : ''
                  "
                >
                  {{ formatNumber(battle.totalFame) }}</span
                >
              </td>
              <td class="px-4 py-2 text-zinc-200 text-xs">
                {{ formatDate(battle.startTime) }}
              </td>
              <td class="px-4 py-2 text-zinc-200 text-xs">
                {{ formatDate(battle.endTime) }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <p class="text-center text-sm text-zinc-900 my-10">@night</p>
    </Container>
  </div>
</template>

<script setup>
const battleEndpoint = "https://api-east.albionbattles.com/battles/";

const allBattlesSexEndpoint1 =
  "https://api-east.albionbattles.com/battles?plyAmount=0&offset=0&search=sex%20with%20ex";
const allBattlesSexEndpoint2 =
  "https://api-east.albionbattles.com/battles?plyAmount=0&offset=20&search=sex%20with%20ex";

const getBattleInfo = await Promise.all([
  useFetch(allBattlesSexEndpoint1),
  useFetch(allBattlesSexEndpoint2),
]);

const offset01 = getBattleInfo[0].data._rawValue.docs;
const offset02 = getBattleInfo[1].data._rawValue.docs;
const battles = offset01.concat(offset02);

console.log(offset01, offset02);

function formatDate(value) {
  const data = new Date(value);

  const opcoesData = {
    timeZone: "America/Sao_Paulo",
    day: "numeric",
    month: "numeric",
    year: "numeric",
  };

  const opcoesHora = {
    timeZone: "America/Sao_Paulo",
    hour: "numeric",
    minute: "numeric",
  };

  const dataFormatada = data.toLocaleDateString("pt-BR", opcoesData);
  const horaFormatada = data.toLocaleTimeString("pt-BR", opcoesHora);

  return `${dataFormatada} às ${horaFormatada}`;
}

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

<style scoped>
div,
p {
  color: white;
}
</style>