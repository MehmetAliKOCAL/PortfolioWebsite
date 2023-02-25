<script setup>
let isHovering = ref(null);

let recentlyPlayedGames = ref(null);
async function getRecentlyPlayedGames() {
  await fetch("https://pwapi.fly.dev/steam/recentlyPlayedGames", {
    method: "GET",
  })
    .then((res) => {
      return res.json();
    })
    .then((text) => {
      return (recentlyPlayedGames.value = text);
    })
    .catch((e) =>
      console.error(
        "ERROR! Can't get the recently played games data from Steam. Error Message: " +
          e
      )
    );
}

let mostPlayedGames = ref(null);
async function getMostPlayedGames() {
  await fetch("https://pwapi.fly.dev/steam/mostPlayedGames", {
    method: "GET",
  })
    .then((res) => {
      return res.json();
    })
    .then((json) => {
      return (mostPlayedGames.value = json);
    })
    .catch((e) =>
      console.error(
        "ERROR! Can't get the recently played games data from Steam. Error Message: " +
          e
      )
    );
}

onBeforeMount(() => {
  getRecentlyPlayedGames();
  getMostPlayedGames();
});

const infoAboutMe =
  "I'm Mehmet, an associate degree 22 years old computer programming student from Turkey. I am currently studying Unity3D, Blender, C#, Vue.js, Nuxt.js, and MySQL. I started my programming journey with front-end web development, by learning HTML, CSS, and JavaScript. After a couple of months, I started learning Vue.js along with the Nuxt.js to advance my front-end skills. But I always wanted to make something related to video games because I love everything about video games and I was always a massive fan of them. So after a year of web development, I find myself in a situation where I had to build something as a school project. While I catch the chance, I started learning Unity to make a video game. And now here I am making games with Unity for fun, using Blender for 3D modeling, and Adobe Photoshop for texturing.";

const proficienciesFirstColumn = [
  // Tailwind.css colors are used for first and second colors
  // Check https://windicss.org/utilities/general/colors for colors

  // Name | First Color | Second Color | Proficiency Level | Glow Color
  ["HTML5", "from-orange-700", "to-orange-500", "w-full", "#f97316"],
  ["CSS3", "from-blue-600", "to-cyan-500", "w-full", "#06b6d4"],
  ["Tailwind CSS", "from-cyan-700", "to-cyan-500", "w-full", "#06b6d4"],
  ["JavaScript", "from-yellow-500", "to-yellow-400", "w-4/5", "#fbbf24"],
  ["Vue.JS", "from-green-600", "to-slate-700", "w-3/5", "#334155"],
  ["Nuxt.JS", "from-green-500", "to-teal-400", "w-3/5", "#2dd4bf"],
];

const proficienciesSecondColumn = [
  // Tailwind.css colors are used for first and second colors
  // Check https://windicss.org/utilities/general/colors for colors

  // Name | First Color | Second Color | Proficiency Level | Glow Color
  ["C#", "from-purple-600", "to-fuchsia-500", "w-3/5", "#d946ef"],
  ["MySQL", "from-blue-600", "to-slate-600", "w-4/5", "#475569"],
  ["Unity3D", "from-black", "to-gray-700", "w-5/10", "#374151"],
  ["Blender", "from-orange-600", "to-orange-400", "w-1/5", "#fb923c"],
  ["Adobe Photoshop", "from-slate-800", "to-sky-500", "w-5/10", "#0ea5e9"],
  [
    "Adobe After Effects",
    "from-blue-800",
    "to-indigo-500",
    "w-5/10",
    "#6366f1",
  ],
];

function calculateLastTimePlayed(seconds) {
  const resultInDays = Math.round((Date.now() / 1000 - seconds) / 60 / 60 / 24);
  if (resultInDays < 1) {
    return "Today";
  } else if (resultInDays < 2) {
    return "Yesterday";
  } else if (resultInDays > 365) {
    return (resultInDays / 365).toFixed(0) + " years ago";
  } else {
    return resultInDays + " days ago";
  }
}

function calculatePlayTime(minutes, shouldBeRounded) {
  if (minutes < 2) {
    return minutes + " minute";
  } else if (minutes < 60) {
    return minutes + " minutes";
  } else if (minutes / 60 < 1.2) {
    return "1 hour";
  } else {
    if (
      (minutes / 60).toFixed(1) % 1 < 0.2 ||
      (minutes / 60).toFixed(1) % 1 >= 0.8 ||
      shouldBeRounded == true
    ) {
      return Math.round(minutes / 60) + " hours";
    } else {
      return (minutes / 60).toFixed(1) + " hours";
    }
  }
}

let showDev = ref(true);
</script>
<template>
  <div
    v-if="showDev"
    class="sticky top-0 bg-slate-800 text-center z-20 text-slate-200 font-bold text-xl py-4 px-2 flex justify-center"
  >
    <div>Still in development</div>
    <div
      @click="showDev = !showDev"
      class="right-5 absolute px-4 cursor-pointer"
    >
      X
    </div>
  </div>
  <div class="min-h-screen min-w-full h-full relative">
    <div class="change-colors absolute h-full min-w-full min-h-full -z-20" />
    <div class="color-wrapper absolute h-full min-w-full min-h-full -z-10" />
    <div class="mx-auto px-40 py-40 text-slate-200/95">
      <section>
        <h1 class="text-5xl font-bold mb-6">Hello</h1>
        <p class="text-3xl font-normal text-slate-400">
          {{ infoAboutMe }}
        </p>
      </section>
      <section>
        <h1 class="text-5xl font-bold mb-6 mt-10">Proficiencies</h1>
        <div class="flex w-full">
          <div class="w-1/2 mr-4">
            <div v-for="item in proficienciesFirstColumn">
              <h2 class="font-semibold text-2xl">{{ item[0] }}</h2>
              <div class="bg-slate-900 rounded-md">
                <div
                  :class="[
                    `bg-gradient-to-r ${item[1]} ${item[2]} ${item[3]} py-5 rounded-md my-3`,
                  ]"
                  :style="[
                    `box-shadow: 20px 0 60px ${item[4]}, -20px 0 60px #000;`,
                  ]"
                />
              </div>
            </div>
          </div>
          <div class="w-1/2 ml-4">
            <div v-for="item in proficienciesSecondColumn">
              <h2 class="font-semibold text-2xl">{{ item[0] }}</h2>
              <div class="bg-slate-900 rounded-md">
                <div
                  :class="[
                    `bg-gradient-to-r ${item[1]} ${item[2]} ${item[3]} py-5 rounded-md my-3`,
                  ]"
                  :style="[
                    `box-shadow: 20px 0 60px ${item[4]}, -20px 0 60px #000;`,
                  ]"
                />
              </div>
            </div>
          </div>
        </div>
      </section>
      <section>
        <h1 class="text-5xl font-bold mb-6 mt-10">Currently Working On</h1>
        <p class="text-3xl font-normal text-slate-400">
          A news website to post about video games,
          <a href="" target="_blank" class="text-cyan-500 font-medium">
            check it!
          </a>
        </p>
      </section>
      <section>
        <h1 class="text-5xl font-bold mb-6 mt-10">Recently Played Games</h1>
        <div v-if="recentlyPlayedGames != null">
          <div
            class="inline-flex w-1/3 transition-all duration-300 p-2"
            v-for="item in recentlyPlayedGames"
          >
            <div class="relative inline-flex overflow-hidden w-full">
              <img
                class="transform transition-all duration-300 w-full"
                :class="{
                  'scale-110 filter blur-[1px]':
                    isHovering == recentlyPlayedGames.indexOf(item),
                  'scale-100 filter blur-[0px]':
                    isHovering != recentlyPlayedGames.indexOf(item),
                }"
                :src="`https://steamcdn-a.akamaihd.net/steam/apps/${item.appid}/header.jpg`"
                :alt="item.name + ' Banner'"
              />
              <a
                @mouseenter="isHovering = recentlyPlayedGames.indexOf(item)"
                @mouseleave="isHovering = null"
                class="absolute w-full h-full flex flex-col justify-center items-center bg-black/70 text-white hover:bg-black/80 active:bg-black/85 transition-all duration-300 cursor-pointer"
                :href="`https://store.steampowered.com/app/${item.appid}/`"
                target="_blank"
              >
                <h2 class="text-2xl font-bold text-center">{{ item.name }}</h2>
                <p class="text-lg font-semibold mt-2 -mb-2">
                  {{
                    calculatePlayTime(item.playtime_2weeks, false) +
                    " in last 2 weeks"
                  }}
                </p>
                <p class="text-lg font-semibold">
                  {{
                    calculatePlayTime(item.playtime_forever, false) + " total"
                  }}
                </p>
              </a>
            </div>
          </div>
        </div>
      </section>
      <section>
        <h1 class="text-5xl font-bold mb-6 mt-10">Most Played Games</h1>
        <div v-if="mostPlayedGames != null">
          <div
            class="inline-flex w-1/3 transition-all duration-300 p-2"
            v-for="item in mostPlayedGames"
          >
            <div class="relative inline-flex overflow-hidden w-full">
              <img
                class="transform transition-all duration-300 w-full"
                :class="{
                  'scale-110 filter blur-[1px]':
                    isHovering == mostPlayedGames.indexOf(item) + 6,
                  'scale-100 filter blur-[0px]':
                    isHovering != mostPlayedGames.indexOf(item) + 6,
                }"
                :src="`https://steamcdn-a.akamaihd.net/steam/apps/${item.appid}/header.jpg`"
                :alt="item.name + ' Icon'"
              />
              <a
                @mouseenter="isHovering = mostPlayedGames.indexOf(item) + 6"
                @mouseleave="isHovering = null"
                class="absolute w-full h-full flex flex-col justify-center items-center bg-black/70 text-white hover:bg-black/80 active:bg-black/85 transition-all duration-300 cursor-pointer"
                :href="`https://store.steampowered.com/app/${item.appid}/`"
                target="_blank"
              >
                <h2 class="text-2xl font-bold text-center">{{ item.name }}</h2>
                <p class="text-lg font-semibold mt-2 -mb-2">
                  {{
                    "Total playtime: " +
                    calculatePlayTime(item.playtime_forever, true)
                  }}
                </p>
                <p class="text-lg font-semibold">
                  {{
                    "Last played: " +
                    calculateLastTimePlayed(item.rtime_last_played)
                  }}
                </p>
              </a>
            </div>
          </div>
        </div>
      </section>
      <section>
        <h1 class="text-5xl font-bold">Contact Me</h1>
      </section>
    </div>
  </div>
</template>
<style>
::selection {
  background-color: rgba(0, 255, 255, 0.8);
  color: white;
}

.color-wrapper {
  background-image: radial-gradient(
    ellipse at left top,
    transparent -100%,
    rgba(10, 10, 10) 40%
  );
}

.change-colors {
  background: linear-gradient(
    -45deg,
    #ff3c00,
    #ff0062,
    #e92c2c,
    #cc00ff,
    #3529db,
    #0051ff,
    #00bbff,
    #00ffc3
  );
  animation: color-change 10s ease infinite;
  background-size: 400% 400%;
}

@keyframes color-change {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
</style>