<template>
  <div class="py-1 px-4 w-full relative max-w-7xl">
    <div
      class="
        max-w-7xl
        form-card
        w-full
        relative
        overflow-hidden
        flex flex-col
        justify-center
      "
    >
      <div class="px-8">
        <div class="gradient-circle w-24 h-24 absolute right-28 -top-16" />
        <div class="pt-2 flex flex-row justify-between">
          <div class="py-2 flex flex-col">
            <div class="flex justify-center items-center">
              <div class="mr-8">
                <div
                  class="w-16 h-16 emoji_part2 flex justify-center items-center"
                >
                  <div
                    class="w-16 h-16 emoji_icon flex justify-center items-center"
                    v-if="emotes.length > 0 && emoteVal.length === 0"
                  >
                    <img class="w-10 h-10" v-bind:src="emotes" />
                  </div>
                  <div
                    class="w-16 h-16 emoji_icon flex justify-center items-center"
                    v-if="emotes.length === 0 && emoteVal.length === 0 && content.icon === null"
                  >
                    <img class="w-10 h-10" v-bind:src="emotesempty" />
                  </div>
                  <div class="w-12 h-12 text-center icon_font" v-else>
                    {{ emoteVal.length ? emoteVal : content.icon }}
                  </div>
                </div>
              </div>
              <div>
                <p class="text-2xl pt-2 pb-1 font-semibold font-montserrat capitalize">
                  {{ title || `Form ${content.ident.substring(4, 5)}` }}
                </p>
                <p class="text-sm pt-1 pb-2">
                  Contains
                  {{
                    content.questions
                      ? content.questions.filter((x) => typeof x === 'string')
                          .length
                      : 0
                  }}
                  description and
                  {{
                    content.questions
                      ? content.questions.filter((x) => typeof x !== 'string')
                          .length
                      : 0
                  }}
                  mcqs based questions
                </p>
              </div>
            </div>
          </div>
          <div class="flex flex-row items-center">
            <div class="p-2">
              <MiscToggle :enabled="content.open" />
            </div>
            <div class="p-2">
              <button
                class="p-2 w-8 h-8 rounded-full bg-gray-600"
                @click="delApplication(content.ident)"
              >
                <SVGRemove size="16" class="text-red" />
              </button>
            </div>
            <div class="p-2" @click="toggle">
              <button v-if="toggled" class="p-2 w-8 h-8">
                <SVGDown
                  size="20"
                  class="text-red transform rotate-180 transition duration-500"
                />
              </button>
              <button v-else class="p-2 w-8 h-8">
                <SVGDown size="20" class="text-red transition duration-500" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      :class="`${
        toggled ? '' : 'hidden'
      } overflow-hidden transition-all flex flex-col justify-start duration-500 ease-in-out -bottom-24 mt-2`"
    >
      <!-- ************************* Test Input, emoji text filed *************************** -->
      <div class="flex justify-between z-10">
        <!-- **************** Text Input ******************** -->
        <div class="mx-2 w-1/2 ml-0">
          <div class="py-2">
            <div class="py-2 flex flex-col lg:flex-row items-center mx-auto">
              <div class="group relative w-full">
                <input
                  v-model="appname"
                  class="
                    border-gray-500 border-opacity-80
                    hover:border-selectorhighlight
                    active:border-selectorhighlight
                    bg-transparent
                    p-2
                    border-2
                    rounded-md
                    w-full
                    h-12
                  "
                  style="margin-left: 1px"
                  @keyup.enter="inputappname(appname, content.ident)"
                />
              </div>
            </div>
          </div>
          <div>
            <div class="form-card w-full relative flex flex-col justify-center overflow-hidden">
              <div class="px-2">
              <div class="gradient-circle w-24 h-24 absolute right-2 top-2" />
                <div class="pt-2 flex flex-row justify-between">
                  <div class="ml-3 py-2 flex flex-col justify-center">
                    <p class="pb-1 text-base font-montserrat">Shuffle</p>
                    <p class="text-xs pt-1 pb-2">
                      Whether to send the dm to the winner or not
                    </p>
                  </div>
                  <div class="flex flex-row items-center">
                    <div class="p-2">
                      <MiscToggle :enabled="content.open" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- ******* Text Area ******* -->
        <div class="mx-2 my-3 w-1/2 mr-0 p-0">
          <div class="form-card" style="backdrop-filter: blur(20px)">
            <div class="p-2">
              <textarea
                class="
                  form-control
                  block
                  w-full
                  px-3
                  py-1.5
                  text-base
                  font-normal
                  text-white
                  bg-clip-padding bg-transparent
                  border border-solid border-transparent
                  rounded
                  transition
                  ease-in-out
                  m-0
                  focus:text-white
                  focus:bg-transparent
                  focus:border-transparent
                  focus:outline-none
                "
                ref="emojides"
                rows="4"
                :value="desdata"
                placeholder="text your description"
                maxlength="1024"
                @input="changetext"
                @keypress="updatetextarea(desdata, content.ident)"
              ></textarea>
              <div class="flex justify-end">
                <div class="cursor-pointer" @click="emojiClickdes">
                  <SVGTextemoji />
                </div>
                <div class="ml-2 mr-4">{{ desnum }}/1024</div>
              </div>
            </div>
            <picker
              v-if="emoteStatedes"
              title="Pick your emoji"
              emoji="thumbsup"
              :autoFocus="true"
              @select="(emoji) => addEmojides(emoji)"
              :style="{ width: '100%', top: '150px' }"
              class="absolute z-10 w-1/2"
            >
            </picker>
          </div>
        </div>
      </div>

      <!-- ************************* Emoji, App logging *************************** -->
      <div class="flex justify-between z-18">
        <div class="form_grad mx-2 my-3 w-1/2 ml-0 px-2 flex items-center">
          <div class="mx-3 my-5 w-full">
            <h6 class="emote_title mb-5">Choose Emote</h6>
            <div class="emo_section flex justify-between w-full relative">
              <div
                class="w-16 h-16 emoji_part1 flex justify-center items-center"
              >
                <div
                    class="w-16 h-16 emoji_part flex justify-center items-center"
                    v-if="emotes.length > 0 && emoteVal.length === 0"
                  >
                    <img class="w-10 h-10" v-bind:src="emotes" />
                  </div>
                  <div
                    class="w-16 h-16 emoji_part flex justify-center items-center"
                    v-if="emotes.length === 0 && emoteVal.length === 0 && content.icon === null"
                  >
                    <img class="w-10 h-10" v-bind:src="emotesempty" />
                  </div>
                  <div class="w-12 h-12 text-center icon_font" v-else>
                    {{ emoteVal.length ? emoteVal : content.icon }}
                  </div>
              </div>

              <div
                class="w-16 h-16 emoji_part flex justify-center items-center"
              >
                <img class="w-10 h-10" v-bind:src="emotes1" />
              </div>
              <div
                class="w-16 h-16 emoji_part flex justify-center items-center"
              >
                <img class="w-10 h-10" v-bind:src="emotes2" />
              </div>
              <div
                class="w-16 h-16 emoji_part flex justify-center items-center"
              >
                <img class="w-10 h-10" v-bind:src="emotes3" />
              </div>
              <div
                class="w-16 h-16 emoji_part flex justify-center items-center"
              >
                <img class="w-10 h-10" v-bind:src="emotes4" />
              </div>
              <div
                @click="emojiClick"
                class="
                  w-16
                  h-16
                  emoji_part
                  flex
                  justify-center
                  items-center
                  emoji_sel
                "
              >
                +
              </div>
              <picker
                v-if="emoteState"
                title="Pick your emoji"
                emoji="thumbsup"
                :autoFocus="true"
                @select="(emoji) => addEmoji(emoji)"
                :style="{ width: '100%', top: '70px' }"
                class="absolute z-10 w-full"
              >
              </picker>
            </div>
          </div>
        </div>
        <!-- **** App logging ****** -->
        <div class="mx-2 my-3 w-1/2 mr-0 p-0">
          <div class="form-card" style="backdrop-filter: blur(20px)">
            <div class="">
              <!-- <div class="w-24 h-24"> -->
                <div class="gradient-circle w-20 h-20 absolute right-2 top-2" />
              <!-- </div> -->
              
              <div class="py-2 flex flex-row">
                <div class="w-full mr-10">
                  <div class="mx-5">
                    <p
                      class="text-base pt-2 pb-1 font-semibold font-montserrat"
                    >
                      Application logging
                    </p>
                    <p class="text-xs pt-1 pb-2">
                      Do you want denied remote role? Change it to a suitable
                      one below!
                    </p>
                  </div>
                  <div class="w-full mx-5 my-5">
                    <div v-if="$fetchState.pending" class="group relative">
                      <div
                        class="
                          border-gray-500 border-opacity-80
                          bg-transparent
                          h-12
                          border-2
                          p-2
                          rounded-md
                        "
                      >
                        <div
                          class="
                            h-full
                            my-auto
                            bg-gray-600
                            rounded
                            animate-pulse
                          "
                        ></div>
                      </div>
                    </div>
                    <div v-else class="group relative">
                      <div
                        id="dropdown"
                        class="
                          rounded-xl
                          bg-transparent
                          border-gray-700 border-2
                          hover:border-selectorhighlight
                          active:border-selectorhighlight
                        "
                      >
                        <div
                          :class="`inset-0 w-full fixed h-full z-17 block ${
                            dropOpen_logging ? 'visible' : 'invisible'
                          }`"
                          @click="toggleOff_logging"
                        />
                        <div>
                          <div
                            id="dropdown-stuff"
                            class="h-12 p-2 relative flex flex-col items-center"
                          >
                            <div
                              id="default-stuff"
                              class="
                                my-auto
                                w-full
                                flex flex-row
                                items-center
                                cursor-pointer
                              "
                              @click="toggleDrop_logging"
                            >
                              <div
                                class="
                                  flex flex-row
                                  h-full
                                  items-center
                                  justify-center
                                "
                              ></div>
                              <div>
                                <div class="text-md font-semibold px-2">
                                  {{ logging_selected.name }}
                                </div>
                              </div>
                              <div class="text-gray-300 pl-1 ml-auto mr-1">
                                <SVGDown
                                  :class="`${
                                    dropOpen_logging
                                      ? 'transform rotate-180 transition duration-500'
                                      : 'transition duration-500'
                                  }`"
                                />
                              </div>
                            </div>
                            <div
                              id="choice-stuff"
                              :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
                                dropOpen_logging ? 'block' : 'hidden'
                              }`"
                            >
                              <div
                                v-for="log in channels"
                                :key="log.id"
                                :class="`flex flex-row items-center ${
                                  log == logging_selected
                                    ? 'bg-discortics-dropselected '
                                    : ''
                                }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
                                @click="switchApplogging(log)"
                              >
                                <div
                                  class="
                                    flex flex-row
                                    h-full
                                    items-center
                                    justify-center
                                    pl-1
                                  "
                                ></div>
                                <div
                                  class="truncate text-md font-semibold px-1"
                                >
                                  {{ log.name }}
                                </div>
                                <div
                                  :class="`text-gray-300 ml-auto${
                                    log == logging_selected ? '' : ' invisible'
                                  }`"
                                >
                                  <SVGDropSelected />
                                </div>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- ************************* Accepted, Denied role *************************** -->
      <div class="flex justify-between z-16">
        <!-- Accepted role -->
        <div class="form-card mx-2 my-3 w-1/2 ml-0">
          <div class="gradient-circle w-20 h-20 absolute right-2 top-2" />
          <div class="m-5 py-2">
            <p class="text-2xl pt-2 pb-1 font-semibold font-montserrat">
              Accepted Role
            </p>
            <p class="text-sm pt-1 pb-2">
              Do you want accept add role? Change it to a suitable one below!
            </p>
          </div>
          <div class="m-5 w-full bottom-2">
            <div class="flex justify-between">
              <div class="w-1/3 mr-5">
                <div v-if="$fetchState.pending" class="group relative">
                  <div
                    class="
                      border-gray-500 border-opacity-80
                      bg-transparent
                      h-12
                      border-2
                      p-2
                      rounded-md
                    "
                  >
                    <div
                      class="h-full my-auto bg-gray-600 rounded animate-pulse"
                    ></div>
                  </div>
                </div>
                <div v-else class="group relative">
                  <div
                    id="dropdown"
                    class="
                      rounded-xl
                      bg-transparent
                      border-gray-700 border-2
                      hover:border-selectorhighlight
                      active:border-selectorhighlight
                    "
                  >
                    <div
                      :class="`inset-0 w-full fixed h-full z-17 block ${
                        dropOpen2 ? 'visible' : 'invisible'
                      }`"
                      @click="accepted1Toggleoff"
                    />
                    <div
                      id="dropdown-stuff"
                      class="h-12 p-2 relative flex flex-col items-center"
                    >
                      <div
                        id="default-stuff"
                        class="
                          my-auto
                          w-full
                          flex flex-row
                          items-center
                          cursor-pointer
                        "
                        @click="toggleDrop2"
                      >
                        <div
                          class="
                            flex flex-row
                            h-full
                            items-center
                            justify-center
                          "
                        ></div>
                        <div v-if="content.accepted">
                          <div class="text-md font-semibold px-2">
                            {{ selected }}
                          </div>
                        </div>
                        <div v-else>
                          <div class="text-md font-semibold px-2"></div>
                        </div>

                        <div class="text-gray-300 pl-1 ml-auto mr-1">
                          <SVGDown
                            :class="`${
                              dropOpen2
                                ? 'transform rotate-180 transition duration-500'
                                : 'transition duration-500'
                            }`"
                          />
                        </div>
                      </div>
                      <div
                        id="choice-stuff"
                        :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
                          dropOpen2 ? 'block' : 'hidden'
                        }`"
                      >
                        <div
                          v-for="lang in langs"
                          :key="lang"
                          :class="`flex flex-row items-center ${
                            lang == selected
                              ? 'bg-discortics-dropselected '
                              : ''
                          }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
                          @click="switchAccepted1(lang)"
                        >
                          <div
                            class="
                              flex flex-row
                              h-full
                              items-center
                              justify-center
                              pl-1
                            "
                          ></div>
                          <div class="truncate text-md font-semibold px-1">
                            {{ lang }}
                          </div>
                          <div
                            :class="`text-gray-300 ml-auto${
                              lang == selected ? '' : ' invisible'
                            }`"
                          >
                            <SVGDropSelected />
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="w-2/3 mr-10">
                <div v-if="$fetchState.pending" class="group relative">
                  <div
                    class="
                      border-gray-500 border-opacity-80
                      bg-transparent
                      h-12
                      border-2
                      p-2
                      rounded-md
                    "
                  >
                    <div
                      class="h-full my-auto bg-gray-600 rounded animate-pulse"
                    ></div>
                  </div>
                </div>
                <div v-else class="group relative">
                  <div
                    id="dropdown"
                    class="
                      rounded-xl
                      bg-transparent
                      border-gray-700 border-2
                      hover:border-selectorhighlight
                      active:border-selectorhighlight
                    "
                  >
                    <div
                      :class="`inset-0 w-full fixed h-full z-17 block ${
                        dropOpenAccepted ? 'visible' : 'invisible'
                      }`"
                      @click="accepted2Toggleoff"
                    />
                    <div
                      id="dropdown-stuff"
                      class="h-12 p-2 relative flex flex-col items-center"
                    >
                      <div
                        id="default-stuff"
                        class="
                          my-auto
                          w-full
                          flex flex-row
                          items-center
                          cursor-pointer
                        "
                        @click="toggleDropAccepted"
                      >
                        <div
                          class="
                            flex flex-row
                            h-full
                            items-center
                            justify-center
                          "
                        ></div>
                        <div class="text-md font-semibold px-2">
                          {{ selectedAccepted.name }}
                        </div>
                        <div class="text-gray-300 pl-1 ml-auto mr-1">
                          <SVGDown
                            :class="`${
                              dropOpenAccepted
                                ? 'transform rotate-180 transition duration-500'
                                : 'transition duration-500'
                            }`"
                          />
                        </div>
                      </div>
                      <div
                        id="choice-stuff"
                        :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
                          dropOpenAccepted ? 'block' : 'hidden'
                        }`"
                      >
                        <div
                          v-for="role in roles"
                          :key="role.id"
                          :class="`flex flex-row items-center ${
                            role == selectedAccepted.name
                              ? 'bg-discortics-dropselected '
                              : ''
                          }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
                          @click="switchAccepted2(role)"
                        >
                          <div
                            class="
                              flex flex-row
                              h-full
                              items-center
                              justify-center
                              pl-1
                            "
                          ></div>
                          <div class="truncate text-md font-semibold px-1">
                            {{ role.name }}
                          </div>
                          <div
                            :class="`text-gray-300 ml-auto${
                              role == selectedAccepted.name ? '' : ' invisible'
                            }`"
                          >
                            <SVGDropSelected />
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- Denied role -->
        <div class="mx-2 my-3 w-1/2 mr-0 p-0">
          <div class="form-card">
            <div class="py-2">
              <div class="gradient-circle w-20 h-20 absolute right-2 top-2" />
              <div class="m-5 py-1">
                <p class="text-2xl pt-2 pb-1 font-semibold font-montserrat">
                  Denied remote role
                </p>
                <p class="text-sm pt-1 pb-2">
                  Do you want denied remote role? Change it to a suitable one
                  below!
                </p>
              </div>
              <div class="m-5">
                <div class="flex justify-between">
                  <div class="w-1/3 mr-3">
                    <div v-if="$fetchState.pending" class="group relative">
                      <div
                        class="
                          border-gray-500 border-opacity-80
                          bg-transparent
                          h-12
                          border-2
                          p-2
                          rounded-md
                        "
                      >
                        <div
                          class="
                            h-full
                            my-auto
                            bg-gray-600
                            rounded
                            animate-pulse
                          "
                        ></div>
                      </div>
                    </div>
                    <div v-else class="group relative">
                      <div
                        id="dropdown"
                        class="
                          rounded-xl
                          bg-transparent
                          border-gray-700 border-2
                          hover:border-selectorhighlight
                          active:border-selectorhighlight
                        "
                      >
                        <div
                          :class="`inset-0 w-full fixed h-full z-17 block ${
                            dropOpen_denied ? 'visible' : 'invisible'
                          }`"
                          @click="toggleOff_denied"
                        />
                        <div
                          id="dropdown-stuff"
                          class="h-12 p-2 relative flex flex-col items-center"
                        >
                          <div
                            id="default-stuff"
                            class="
                              my-auto
                              w-full
                              flex flex-row
                              items-center
                              cursor-pointer
                            "
                            @click="toggleDrop_denied"
                          >
                            <div
                              class="
                                flex flex-row
                                h-full
                                items-center
                                justify-center
                              "
                            ></div>
                            <div v-if="content.denied">
                              <div class="text-md font-semibold px-2">
                                {{ denied_selected }}
                              </div>
                            </div>
                            <div class="text-gray-300 pl-1 ml-auto mr-1">
                              <SVGDown
                                :class="`${
                                  dropOpen_denied
                                    ? 'transform rotate-180 transition duration-500'
                                    : 'transition duration-500'
                                }`"
                              />
                            </div>
                          </div>
                          <div
                            id="choice-stuff"
                            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
                              dropOpen_denied ? 'block' : 'hidden'
                            }`"
                          >
                            <div
                              v-for="langdenied in langsdenied"
                              :key="langdenied"
                              :class="`flex flex-row items-center ${
                                langdenied == denied_selected
                                  ? 'bg-discortics-dropselected '
                                  : ''
                              }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
                              @click="switchDenied1(langdenied)"
                            >
                              <div
                                class="
                                  flex flex-row
                                  h-full
                                  items-center
                                  justify-center
                                  pl-1
                                "
                              ></div>
                              <div class="truncate text-md font-semibold px-1">
                                {{ langdenied }}
                              </div>
                              <div
                                :class="`text-gray-300 ml-auto${
                                  langdenied == denied_selected
                                    ? ''
                                    : ' invisible'
                                }`"
                              >
                                <SVGDropSelected />
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="w-2/3">
                    <div v-if="$fetchState.pending" class="group relative">
                      <div
                        class="
                          border-gray-500 border-opacity-80
                          bg-transparent
                          h-12
                          border-2
                          p-2
                          rounded-md
                        "
                      >
                        <div
                          class="
                            h-full
                            my-auto
                            bg-gray-600
                            rounded
                            animate-pulse
                          "
                        ></div>
                      </div>
                    </div>
                    <div v-else class="group relative">
                      <div
                        id="dropdown"
                        class="
                          rounded-xl
                          bg-transparent
                          border-gray-700 border-2
                          hover:border-selectorhighlight
                          active:border-selectorhighlight
                        "
                      >
                        <div
                          :class="`inset-0 w-full fixed h-full z-17 block ${
                            dropOpenDenied ? 'visible' : 'invisible'
                          }`"
                          @click="toggleOffDenied"
                        />
                        <div
                          id="dropdown-stuff"
                          class="h-12 p-2 relative flex flex-col items-center"
                        >
                          <div
                            id="default-stuff"
                            class="
                              my-auto
                              w-full
                              flex flex-row
                              items-center
                              cursor-pointer
                            "
                            @click="toggleDropDenied"
                          >
                            <div
                              class="
                                flex flex-row
                                h-full
                                items-center
                                justify-center
                              "
                            ></div>
                            <div class="text-md font-semibold px-2">
                              {{ selectedDenied.name }}
                            </div>
                            <div class="text-gray-300 pl-1 ml-auto mr-1">
                              <SVGDown
                                :class="`${
                                  dropOpenDenied
                                    ? 'transform rotate-180 transition duration-500'
                                    : 'transition duration-500'
                                }`"
                              />
                            </div>
                          </div>
                          <div
                            id="choice-stuff"
                            :class="`z-30 bg-gradient-to-l border border-gray-600 rounded-xl w-full divide-y divide-gray-600 h-32 overflow-y-scroll from-discortics-header via-discortics-header to-discortics-header absolute flex flex-col items-start -bottom-32 ${
                              dropOpenDenied ? 'block' : 'hidden'
                            }`"
                          >
                            <div
                              v-for="denied in roles"
                              :key="denied.id"
                              :class="`flex flex-row items-center ${
                                denied == selectedDenied
                                  ? 'bg-discortics-dropselected '
                                  : ''
                              }hover:bg-gray-800 px-1 py-2 w-full cursor-pointer`"
                              @click="
                                denied == selectedDenied
                                  ? (x) => {}
                                  : switchDenied2(denied)
                              "
                            >
                              <div
                                class="
                                  flex flex-row
                                  h-full
                                  items-center
                                  justify-center
                                  pl-1
                                "
                              ></div>
                              <div class="truncate text-md font-semibold px-1">
                                {{ denied.name }}
                              </div>
                              <div
                                :class="`text-gray-300 ml-auto${
                                  denied == selectedDenied ? '' : ' invisible'
                                }`"
                              >
                                <SVGDropSelected />
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- White, Blacklisting -->
      <div class="mx-2 w-full ml-0 mb-24 z-14">
        <div>
          <div
            class="form-card w-full relative flex flex-col justify-center py-3"
          >
            <div class="px-2">
              <div class="pt-2 flex flex-row justify-between">
                <div class="ml-3 py-2 flex flex-col justify-center">
                  <p class="pb-1 text-base font-montserrat">
                    Whitelisting & Blacklisting
                  </p>
                </div>
                <div class="flex flex-row items-center">
                  <div class="pb-2 mr-5">
                    <button class="rounded manage" @click="showManage">
                      Manage
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Questions part -->
      <div class="flex flex-col items-stretch space-y-4 w-full">
        <div class="text-2xl">Questions</div>
        <div
          v-for="(question, index) in content.questions"
          :key="index"
          class="relative"
        >
          <div
            class="
              w-full
              h-16
              items-center
              bg-question
              flex flex-row
              justify-between
              bg-question_grad
              overflow-hidden
              rounded-xl
            "
          >
            <div class="flex flex-row p-2 w-full">
              <div
                class="
                  absolute
                  left-0
                  top-0
                  bg-indigo-400
                  rounded-xl
                  h-16
                  w-10
                  flex flex-row
                  items-center
                  justify-center
                "
              >
                <span class="text-xl">{{ index + 1 }}</span>
              </div>
              <div class="ml-10 text-question_color w-full">
                <div class="group relative w-full">
                  <input
                    class="
                      outline-none
                      bg-transparent
                      p-2
                      border-none
                      w-full
                      h-12
                    "
                    style="margin-left: 1px"
                    :v-model="
                      typeof question === 'string'
                        ? question
                        : typeof question.question === undefined
                        ? ''
                        : question.question
                    "
                    :value="
                      typeof question === 'string'
                        ? question
                        : typeof question.question === undefined
                        ? ''
                        : question.question
                    "
                    @keyup.enter="inputQuestion($event, index)"
                  />
                </div>
              </div>
            </div>
            <span class="text-xl absolute right-10">
              <SVGDelete size="14" class="text-red transition duration-500" />
            </span>
            <button
              class="
                bg-indigo-400
                rounded-xl
                h-16
                w-24
                flex flex-row
                items-center
                justify-center
                bg-question_closebtn
                backdrop-blur-xl
              "
              style="-webkit-filter: blur(24px)"
              @click="ondelQuestion(index)"
            ></button>
          </div>

          <div v-if="(typeof question !== 'string')" class="flex justify-between">
            <div class="top-8 rounded items-center flex flex-row">
              <SVGArrow size="20" class="mx-4 my-1" />
              <input-tag
                v-if="tagque === question"
                v-model="question.options"
                class="inputtag mt-6 w-full"
                validate="text"
                :read-only="false"
              ></input-tag>
              <input-tag
                v-else
                v-model="question.options"
                class="inputtag mt-6 w-full"
                validate="text"
                :read-only="true"
              ></input-tag>
            </div>
            <div class="top-8 rounded items-center flex flex-row">
              <button
                class="
                  rounded
                  w-32
                  h-10
                  bg-question_addmcq
                  mt-6
                  text-xs
                  text-question_addmcqtext
                  items-center
                  flex
                  justify-center
                  font-medium
                  text-center
                  mr-2
                "
                @click="addoption(question)"
              >
                + Add Option
              </button>
              <button
                class="
                  rounded
                  w-32
                  h-10
                  bg-question_reset
                  mt-6
                  text-xs
                  text-question_resettext
                  items-center
                  flex
                  font-medium
                  justify-center
                "
                @click="resetquestion(question)"
              >
                Reset Question
              </button>
            </div>
          </div>
        </div>
        <div class="flex justify-between">
          <div class="mx-2 w-1/2 ml-0">
            <div class="py-2 flex flex-col">
              <button
                type="button"
                class="
                  inline-flex
                  items-center
                  px-6
                  py-3
                  border-dashed border-2 border-gray-500
                  rounded-xl
                  justify-center
                "
                @click="onaddQuestion"
              >
                <div>
                  <SVGPluss class="mb-px" />
                </div>
                &nbsp; Add a question
              </button>
            </div>
          </div>
          <div class="mx-2 w-1/2 ml-0">
            <div class="py-2 flex flex-col">
              <button
                type="button"
                class="
                  inline-flex
                  items-center
                  px-6
                  py-3
                  border-dashed border-2 border-gray-500
                  rounded-xl
                  justify-center
                "
                @click="onaddMcq"
              >
                <div>
                  <SVGPluss class="mb-px" />
                </div>
                &nbsp; Add a MCQ
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <Responsesmodal
      :modaldata="modaldata"
      v-show="showModal"
      @close-modal="showModal = false"
    />
    <Acceptmodal v-show="showModal1" @close-modal1="showModal1 = false" />
    <Managemodal
      :managemodaldata="content"
      :channeldata="channels"
      :roledata="roles"
      v-show="showModal2"
      @close-modal2="showModal2 = false"
    />
    <PopupModal
      :ident="appident"
      :oldval="appoldval"
      :newval="appnewval"
      :type="apptype"
      v-show="showModal3"
      @submitdata="submitdatas"
      @close-modal3="showModal3 = false"
    />
  </div>
</template>

<script>
import { Picker } from 'emoji-mart-vue'
import InputTag from 'vue-input-tag'
import Vue from 'vue'
import Responsesmodal from '~/components/Modal/responsesmodal.vue'
import Acceptmodal from '~/components/Modal/Acceptmodal.vue'
import Managemodal from '~/components/Modal/ManageModal.vue'
import PopupModal from '~/components/Modal/PopupModal.vue'
Vue.component('picker', Picker)
Vue.component('input-tag', InputTag)
export default {
  name: 'IndexNew',
  components: {
    Responsesmodal,
    Acceptmodal,
    Managemodal,
    PopupModal
  },
  data() {
    const langs = ['add', 'remove']
    const langsdenied = ['add', 'remove']
    const showModal = false
    const showModal1 = false
    const showModal2 = false
    const showModal3 = false
    const modaldata = []
    const appname = ''
    const emotes = []
    const emotes1 = []
    const emotes2 = []
    const emotes3 = []
    const emotes4 = []
    const emotesempty = []
    const emoteState = false
    const emoteStatedes = false
    const emoteVal = ''
    const oldemoteVal = ''
    const desdata = ''
    const olddesdata = ''
    const desnum = 0
    const tagque = {}
    const oldappname = ''
    const appnameNewval = ''
    const appdesNewval = ''
    const olddata = ''
    const newdata = ''
    const appident = ''
    const appoldval = ''
    const appnewval = ''
    const apptype = ''
    const appquestion = []
    return {
      appquestion,
      langs,
      langsdenied,
      selected: langs[0],
      oldSelected: langs[0],

      selectedAccepted: this.roles[0],
      oldSelectedAccepted: this.roles[0],

      selectedDenied: this.roles[0],
      oldSelectedDenied: this.roles[0],

      denied_selected: langs[0],
      logging_selected: [],
      oldlogging_selected: [],
      denied_oldSelected: langs[0],

      dropOpen2: false,
      dropOpenAccepted: false,
      dropOpenDenied: false,
      dropOpen_denied: false,
      dropOpen_logging: false,

      toggled: false,
      toggled_sub: false,
      showModal,
      showModal1,
      showModal2,
      showModal3,
      modaldata,
      appname,
      oldappname,
      emotes,
      emotes1,
      emotes2,
      emotes3,
      emotes4,
      emotesempty,
      emoteState,
      emoteStatedes,
      emoteVal,
      oldemoteVal,
      desdata,
      olddesdata,
      desnum,
      tagque,
      appnameNewval,
      appdesNewval,
      olddata,
      newdata,
      appident,
      appoldval,
      appnewval,
      apptype
    }
  },
  fetch() {
    let lang = this.langs[0]
    // ************* accepted *************
    if (!this.content.accepted) {
      this.selected = this.langs[0]
      lang = ''
    } else {
      lang = this.content.accepted.action
    }
    this.selected = this.langs.find((item) => item === lang)
    this.oldSelected = this.selected

    // *************** denied *****************
    let langdenied = this.langsdenied[0]
    if (!this.content.denied) {
      this.denied_selected = this.langsdenied[0]
      langdenied = ''
    } else {
      langdenied = this.content.denied.action
    }
    this.denied_selected = this.langsdenied.find((item) => item === langdenied)
    this.denied_oldSelected = this.denied_selected

    // *************** logging *****************
    if (this.content.auto) {
      this.channels.map((item, index) => {
        if (item.id === this.content.auto) {
          this.logging_selected = item
          this.oldlogging_selected = this.logging_selected
        }
        return item
      })
    } else {
      this.logging_selected = { id: '111', name: 'disabled' }
      this.oldlogging_selected = this.logging_selected
    }

    // ******************* accepted role ***********************
    if (!this.content.accepted) {
      this.selectedAccepted = ''
    } else {
      this.roles.map((item, index) => {
        if (item.id === this.content.accepted.role) {
          this.selectedAccepted = this.roles[index]
        }
        return this.oldSelectedAccepted
      })
    }
    this.oldSelectedAccepted = this.selectedAccepted

    // ******************* denied role ***********************
    if (!this.content.denied) {
      this.selectedDenied = ''
    } else {
      this.roles.map((item, index) => {
        if (item.id === this.content.denied.role) {
          this.selectedDenied = this.roles[index]
        }
        return this.selectedDenied
      })
    }
    this.oldSelectedDenied = this.selectedDenied
    this.appname = this.content.name
    this.oldappname = this.appname
    this.appquestion = this.content.questions
  },
  mounted() {
    const token = localStorage.getItem('sessionToken')
    fetch(
      'https://api.discortics.com/v2/emotes/' + localStorage.getItem('guildID'),
      {
        method: 'get',
        headers: { Authorization: `Bearer ${token}` },
      }
    )
      .then((response) => response.json())
      .then((responseJson) => {
        const data = responseJson.emotes
        data.map((item, index) => {
          if (item.id === this.content.icon) {
            this.emotes = item.icon
          }
          return this.emotes
        })
        this.emotesempty = responseJson.emotes[0].icon
        this.emotes1 = data[Math.floor(Math.random() * data.length)].icon
        this.emotes2 = data[Math.floor(Math.random() * data.length)].icon
        this.emotes3 = data[Math.floor(Math.random() * data.length)].icon
        this.emotes4 = data[Math.floor(Math.random() * data.length)].icon
      })
      .catch((error) => {
        console.log(error)
      })

    if (this.content.description) {
      this.desdata = this.content.description
      this.olddesdata = this.desdata
      this.desnum = this.content.description.length
    }
    if (this.content.icon) {
      if (this.content.icon.length === 18) {
        this.oldemoteVal = this.emotes
      } else {
        this.oldemoteVal = this.content.icon
      }
    }
  },
  props: {
    title: {
      type: String,
      default: null,
    },
    roles: {
      type: Array,
      default: null,
    },
    channels: {
      type: Array,
      default: null,
    },
    content: {
      type: Object,
      default: null,
    },
    responses: {
      type: Object,
      default: null,
    },
  },
  methods: {
    toggle() {
      this.toggled = !this.toggled
    },
    toggle_sub() {
      this.toggled_sub = !this.toggled_sub
    },
    accepted1Toggleoff() {
      this.dropOpen2 = false
    },
    toggleDrop2() {
      this.dropOpen2 = !this.dropOpen2
    },
    accepted2Toggleoff() {
      this.dropOpenAccepted = false
    },
    toggleDropAccepted() {
      this.dropOpenAccepted = !this.dropOpenAccepted
    },
    toggleOff_denied() {
      this.dropOpen_denied = false
    },
    toggleDrop_denied() {
      this.dropOpen_denied = !this.dropOpen_denied
    },
    toggleOff_logging() {
      this.dropOpen_logging = false
    },
    toggleDrop_logging() {
      this.dropOpen_logging = !this.dropOpen_logging
    },
    toggleOffDenied() {
      this.dropOpenDenied = false
    },
    toggleDropDenied() {
      this.dropOpenDenied = !this.dropOpenDenied
    },
    switchApplogging(langdenied) {
      if (this.oldlogging_selected !== langdenied) {
        this.logging_selected = langdenied
        this.appident = this.content.ident
        this.appoldval = this.oldlogging_selected
        this.appnewval = langdenied
        this.apptype = 'applogging'
        this.showModal3 = true
      }
      this.toggleOff_logging()
    },
    switchAccepted1(role) {
      if (this.oldSelected !== role) {
        this.selected = role
        this.appident = this.content.ident
        this.appoldval = this.oldSelected
        this.appnewval = role
        this.apptype = 'appaccepted1'
        this.showModal3 = true
      }
      this.accepted1Toggleoff()
    },
    switchAccepted2(role) {
      if (this.oldSelectedAccepted !== this.role) {
        this.selectedAccepted = role
        this.appident = this.content.ident
        this.appoldval = this.oldSelectedAccepted
        this.appnewval = role
        this.apptype = 'appaccepted2'
        this.showModal3 = true
      }
      this.accepted2Toggleoff()
    },
    switchDenied1(denied) {
      if (denied !== this.denied_oldSelected) {
        this.denied_selected = denied
        this.appident = this.content.ident
        this.appoldval = this.denied_oldSelected
        this.appnewval = denied
        this.apptype = 'appdenied1'
        this.showModal3 = true
      }
      this.toggleOff_denied()
    },
    switchDenied2(denied) {
      if (this.oldSelectedDenied !== denied) {
        this.selectedDenied = denied
        this.appident = this.content.ident
        this.appoldval = this.oldSelectedDenied
        this.appnewval = denied
        this.apptype = 'appdenied2'
        this.showModal3 = true
      }
      this.toggleOffDenied()
    },
    showeyeModal(data) {
      if (data !== undefined) {
        this.modaldata = data
        this.showModal = true
      }
    },
    showeyeModal1() {
      this.showModal1 = true
    },
    showManage() {
      this.showModal2 = true
    },
    addEmoji(emoji) {
      if (this.oldemoteVal !== emoji.native) {
        this.emoteVal = emoji.native
        this.appident = this.content.ident
        this.appoldval = this.oldemoteVal
        this.appnewval = emoji.native
        this.apptype = 'appemoji'
        this.showModal3 = true
      }
      this.emoteState = false
    },
    addEmojides(emoji) {
      this.emoteValdes = emoji.native
      const desvalue = this.$refs.emojides.value
      this.desdata = desvalue + this.emoteValdes
      this.desnum = this.desdata.length
      this.emoteStatedes = false
    },
    emojiClick() {
      if (this.emoteState) {
        this.emoteState = false
      } else {
        this.emoteState = true
      }
    },
    emojiClickdes() {
      if (this.emoteStatedes) {
        this.emoteStatedes = false
      } else {
        this.emoteStatedes = true
      }
    },
    onaddQuestion() {
      const questionData = this.content.questions
      questionData.push('')
    },
    onaddMcq() {
      const questionData = this.content.questions
      const objdata = {
        question: '',
        options: [],
        min: 1,
        max: 1,
      }
      questionData.push(objdata)
    },
    ondelQuestion(num) {
      const questionData = this.content.questions
      questionData.splice(num, 1)
      const localdata = localStorage.getItem('localappdata')
      const initdata = JSON.parse(localdata)
      const olddata = initdata[this.content.ident].questions
      this.appident = this.content.ident
      this.appoldval = olddata
      this.appnewval = questionData
      this.apptype = 'delquestion'
      this.showModal3 = true
      
    },
    changetext() {
      const desvalue = this.$refs.emojides.value
      this.desdata = desvalue
      this.desnum = this.desdata.length
    },
    addoption(que) {
      this.tagque = que
    },
    resetquestion(que) {
      let cnt = -1
      this.content.questions.map((item, index) => {
        if (item === que) {
          cnt = index
        }
        return cnt
      })
      if (cnt >= 0) {
        const data = {
          question: '',
          options: [],
          max: 1,
          min: 1,
        }
        this.tagque = que
        const arrdata = this.content.questions
        arrdata[cnt] = data
      }
    },
    delApplication(deldata) {
      const localdata = localStorage.getItem('localappdata')
      const initdata = JSON.parse(localdata)
      delete initdata[deldata]
      const keys = Object.keys(initdata)
      const newarr = []
      keys.map((item, index) => {
        newarr.push('form' + (index + 1))
        return newarr
      })
      const values = Object.values(initdata)
      const result = {}
      newarr.forEach((key, i) => (result[key] = values[i]))
      const senddata = { data: result }
      this.sendData(senddata)
    },
    async sendData(data) {
        const response = await this.$api.request({
        url: `/v2/application/${localStorage.getItem('guildID')}`,
        method: 'post',
        headers: {
          Authorization: `Bearer ${localStorage.getItem('sessionToken')}`,
        },
        data: JSON.stringify(data),
      })
      if (response.status === 200){
        // alert('submit')
      }
    },
    inputappname(newvalue, ident) {
      if (this.oldappname !== newvalue) {
        this.appnameNewval = newvalue
        this.appident = ident
        this.appoldval = this.oldappname
        this.appnewval = newvalue
        this.apptype = 'appname'
        this.showModal3 = true
      }
    },
    updatetextarea(text, ident) {
      if (this.olddesdata !== text) {
        this.desdata = text
        this.appident = ident
        this.appoldval = this.olddesdata
        this.appnewval = text
        this.apptype = 'appdes'
        this.showModal3 = true
      }
    },
    submitdatas(value) {
      if(value.type === 'appname') {
        this.oldappname = value.oldval        
        this.appname = this.oldappname
      } else if(value.type === 'appdes') {
        this.olddesdata = value.oldval
        this.desdata = this.olddesdata
      } else if(value.type === 'applogging') {
        this.oldlogging_selected = value.oldval
        this.logging_selected = this.oldlogging_selected
      } else if(value.type === 'appemoji') {
        this.oldemoteVal = value.oldval
        this.emoteVal = this.oldemoteVal
      } else if(value.type === 'appaccepted1') {
        this.oldSelected = value.oldval
        this.selected = this.oldSelected
      } else if(value.type === 'appaccepted2') {
        this.oldSelectedAccepted = value.oldval
        this.selectedAccepted = this.oldSelectedAccepted
      } else if(value.type === 'appdenied1') {
        this.denied_oldSelected = value.oldval
        this.denied_selected = this.denied_oldSelected
      } else if(value.type === 'appdenied2') {   
        this.oldSelectedDenied = value.oldval
        this.selectedDenied = this.oldSelectedDenied
      } else if(value.type === 'appquestion') {   
        this.olddata = value.oldval
        this.newdata = this.olddata
      } else if(value.type === 'delquestion') {   
        // let questionData = this.content.questions
        // const localdata = localStorage.getItem('localappdata')
        // const initdata = JSON.parse(localdata)
        // const data = initdata[this.content.ident].questions
        // questionData = data
      }
    },
    inputQuestion(event, index) {
      this.olddata = this.content.questions[index]
      let realdata = ''
      if (this.olddata !== event.target.value) {
        this.newdata = event.target.value
        if (typeof this.olddata === 'string') {
          realdata = this.newdata
        } else if (typeof this.olddata === 'undefined') {
          realdata = ''
        } else {
          realdata = {
            question: this.newdata,
            options: [],
            min: 1,
            max: 1,
          }
        }
        const localdata = localStorage.getItem('localappdata')
        const initdata = JSON.parse(localdata)
        initdata[this.content.ident].questions[index] = realdata
        this.appident = this.content.ident
        this.appoldval = this.olddesdata
        this.appnewval = initdata[this.content.ident].questions
        this.apptype = 'appquestion'
        this.showModal3 = true
      }
    },
  },
}
</script>
<style>
.modal-vue .overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal-vue .modal {
  position: relative;
  width: 300px;
  z-index: 9999;
  margin: 0 auto;
  padding: 20px 30px;
  background-color: #fff;
}
.modal-vue .close {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>