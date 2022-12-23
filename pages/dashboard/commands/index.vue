<template>
  <div class="w-full mx-auto">
    <div class="flex flex-col items-start justify-start space-y-8 w-full">
      <div class="py-2 w-full">
        <div class="flex flex-col w-full space-y-8 items-stretch justify-start">
          <div class="py-2 w-full">
            <div class="p-4 flex flex-col items-start space-y-1">
              <span
                class="
                  text-2xl
                  bg-navCurrent
                  text-transparent
                  bg-clip-text
                  font-bold font-montserrat
                "
                >Commands</span
              >
              <span class="text-sm font-quicksand text-graye1">Manage your commmands here!</span>
            </div>
          </div>
          <div
            v-for="app in apps"
            :key="app.ident"
            class="flex flex-col w-full"
          >
            <CommandsCategory
              :title="app.ident"
              :content="app"
              :roles="roles"
              :channels="channels"
            />
          </div>
          
        </div>
      </div>
      <hr class="border-discortics-header w-full" />
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $api, $_ }) {
    const token = localStorage.getItem('sessionToken')
    // let apps = await $api.$request({
    //   url: `v2/application/${localStorage.getItem('guildID')}`,
    //   method: 'get',
    //   headers: { Authorization: `Bearer ${token}` },
    // })
    // const appdata = apps.data
    // localStorage.setItem("localappdata", JSON.stringify(appdata))
    // apps = apps.data
    // const appsxx = Object.keys(apps)
    // const appsyy = Object.values(apps)
    // const appsx = appsxx
    // const appsy = appsyy
    // for (let i = 0; i < appsy.length; ++i) {
    //   appsy[i].ident = appsx[i]
    // }
    const apps = {
      "anime": {
        "angry": {},
        "asearch": {},
        "baka": {},
        "bite": {},
        "blush": {},
        "bored": {},
        "bully": {},
        "character": {},
        "chase": {},
        "cheer": {},
        "confused": {},
        "cry": {},
        "cuddle": {},
        "dance": {},
        "facepalm": {},
        "feed": {},
        "happy": {},
        "high-five": {},
        "hold": {},
        "hug": {},
        "kiss": {},
        "laugh": {},
        "lick": {},
        "manga": {},
        "pat": {},
        "poke": {},
        "revive": {},
        "sad": {},
        "slap": {},
        "sleepy": {},
        "smug": {},
        "tickle": {}
      },
      "application": {
        "accept": {},
        "app-setting": {},
        "app-status": {},
        "apply": {},
        "deny": {},
        "review": {},
        "scroll": {}
      },
      "autopilot": {
        "antiflood": {},
        "antilink": {},
        "antiraid": {},
        "antispam": {},
        "antimention": {},
        "profanity": {},
        "self-bot": {}
      },
      "discover": {
        "bump": {},
        "discovery": {}
      },
      "economy": {
        "among-us": {},
        "balance": {},
        "blackjack": {},
        "daily": {},
        "deposit": {},
        "economy-setting": {},
        "leaderboard": {},
        "rob": {},
        "roulette": {},
        "share": {},
        "shop": {},
        "slot": {},
        "withdraw": {},
        "work": {}
      },
      "family": {
        "disabled" : true,
        "divorce": {},
        "marriage": {},
        "marry": {}
      },
      "fun": {
        "8ball": {},
        "afk": {},
        "color": {},
        "define": {},
        "hack": {},
        "insult": {},
        "joke": {},
        "meme": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [],
            "denied": []
          },
          "channels": [
            "847785687356669973",
            "847787762785714177",
            "847788130224308264",
            "848101949576052768",
            "848105735065567254",
            "848105760219201536",
            "848105805941309450",
            "848105835736465408",
            "848105854322212865",
            "848108415821938718",
            "849896858507018240",
            "849904756759068714",
            "849906910107009075",
            "851516093041606667",
            "853279873774125086",
            "857104413603790878"
          ]
        },
        "minecraft": {},
        "poll": {},
        "say-as": {},
        "topic": {},
        "trump": {}
      },
      "gifs": {
        "bgremove": {},
        "colorise": {},
        "identify": {},
        "obama": {},
        "ocr": {},
        "pet": {}
      },
      "giveaway": {
        "gconfig": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "gdrop": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "gend": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "glist": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "greroll": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "gschedule": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358"
            ],
            "denied": []
          },
          "channels": []
        },
        "gstart": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [
              "851515508079329358",
              "848108676869914694"
            ],
            "denied": []
          },
          "channels": []
        }
      },
      "global": {
        "gbalance": {},
        "gdaily": {},
        "profile": {}
      },
      "info": {
        "avatar": {},
        "banner": {},
        "bot-info": {},
        "crypto": {},
        "discordjs": {},
        "discordpy": {},
        "help": {},
        "invites": {
          "users": {
            "allowed": [],
            "denied": []
          },
          "roles": {
            "allowed": [],
            "denied": []
          },
          "channels": [
            "853279873774125086"
          ]
        },
        "members": {},
        "messages": {},
        "oldest": {},
        "ping": {},
        "premium": {},
        "server": {},
        "shards": {},
        "snipe": {},
        "support": {},
        "time-difference": {},
        "translate": {},
        "user": {},
        "vote": {},
        "youngest": {}
      },
      "levels": {
        "rank": {},
        "xp": {}
      },
      "moderation": {
        "ban": {},
        "kick": {},
        "lockdown": {},
        "mute": {},
        "nickname": {},
        "nuke": {},
        "purge": {},
        "remove-warns": {},
        "slowmode": {},
        "temp-ban": {},
        "timeout": {},
        "unban": {},
        "unlockdown": {},
        "unmute": {},
        "untimeout": {},
        "warn": {},
        "warns": {}
      },
      "music": {
        "autoplay": {},
        "bassboost": {},
        "forward": {},
        "loop": {},
        "lyrics": {},
        "nightcore": {},
        "now-playing": {},
        "pause": {},
        "play": {},
        "previous": {},
        "queue": {},
        "rewind": {},
        "shuffle": {},
        "skip": {},
        "speed": {},
        "stop": {},
        "volume": {},
        "youtube": {}
      },
      "notification": {
        "notify": {}
      },
      "reactionrole": {
        "sr-create": {},
        "sr-delete": {},
        "sr-list": {}
      },
      "settings": {
        "auto-role": {},
        "command": {},
        "confession": {},
        "emote": {},
        "language": {},
        "mod-log": {},
        "muted-role": {},
        "permission": {},
        "prefix": {},
        "reset": {},
        "starboard": {},
        "suggestion": {},
        "tag": {},
        "welcome": {}
      },
      "tickets": {
        "tadd": {},
        "tclaim": {},
        "tclose": {},
        "tmove": {},
        "transcript": {},
        "tremove": {},
        "trename": {},
        "tsend": {},
        "tsetup": {}
      },
      "verification": {
        "verify": {},
        "v-info": {},
        "v-manual": {}
      }
    }
    const formReference1 = {
      disabled: false,
    }

    const formReference = {
      disabled: false,
      users: {
        allowed : [],
        denied : []
      },
      roles : {
        allowed:[],
        denied : []
      },
      channels :[]
    }
    for(const form in apps){
      const data2 = apps[form];
      for(const k in formReference1){
        if(data2[k] === undefined) {
          apps[form][k] = formReference1[k];
        }
      }
      for(const formdata in data2) {
        for(const index in formReference) {
          if(typeof data2[formdata] !== 'boolean' && typeof data2[formdata] !== 'string' && data2[formdata][index] === undefined) {
            apps[form][formdata][index] = formReference[index]
          }
        }
      }
    }
    console.log('-----command data', apps)

    // for(const form in result){
    //     const data2 = result[form];
    //     for(const k in formReference){
    //         if(data2[k] === undefined) {
    //           result[form][k] = formReference[k];
    //         }
    //     }
    // }



    localStorage.setItem("localcommanddata", JSON.stringify(apps))
    const appsx = Object.keys(apps)
    const appsy = Object.values(apps)
    for (let i = 0; i < appsy.length; ++i) {
      appsy[i].ident = appsx[i]
    }
    console.log('appsx=>', appsx)
    console.log('appsy=>', appsy)
    const roles = await $api.$request({
      url: `v2/roles/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
    const channels = await $api.$request({
      url: `v2/channels/${localStorage.getItem('guildID')}`,
      method: 'get',
      headers: { Authorization: `Bearer ${token}` },
    })
   
    return {
      apps: appsy,
      roles: roles.roles,
      channels: channels.channels,
      appsx,
      appsy,
    }
  },
  methods: {
    
  },
  layout: 'dashboard',
}
</script>
