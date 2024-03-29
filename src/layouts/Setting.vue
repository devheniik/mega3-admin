
<template>
  <div>
    <Disclosure
      as="div"
      class="relative bg-blue-700 pb-32 overflow-hidden"
      
      v-slot="{ open }"
    >
    
      <nav
        :class="[
          open ? 'bg-blue-900' : 'bg-transparent',
          'relative z-10 border-b border-teal-500 border-opacity-25 lg:bg-transparent lg:border-none',
        ]"
      >
     
        <div class="max-w-7xl mx-auto px-2 sm:px-4 lg:px-8">
          <div
            class="relative h-16 flex items-center justify-between lg:border-b lg:border-blue-800"
          >
            <div class="px-2 flex items-center lg:px-0">
              <div class="flex-shrink-0">
                <img
                  class="block h-8 w-auto"
                  src="https://mega3-admin-dev-eugen.vercel.app/img/logo_header.png"
                  alt="mega3"
                />
              </div>
              <div class="hidden lg:block lg:ml-6 lg:space-x-4">
                <div class="flex">
                  <a
                    v-for="item in navigation"
                    :key="item.name"
                    :href="item.href"
                    :class="[
                      item.current
                        ? 'bg-black bg-opacity-25'
                        : 'hover:bg-blue-800',
                      'rounded-md py-2 px-3 text-sm font-medium text-white',
                    ]"
                    >{{ item.name }}</a
                  >
                </div>
              </div>
            </div>
            <div class="flex-1 px-2 flex justify-center lg:ml-6 lg:justify-end">
              <div class="max-w-lg w-full lg:max-w-xs">
                <label for="search" class="sr-only">Search</label>
                <div
                  class="relative text-blue-100 focus-within:text-gray-400"
                >
                  <div
                    class="pointer-events-none absolute inset-y-0 left-0 pl-3 flex items-center"
                  >
                    <SearchIcon
                      class="flex-shrink-0 h-5 w-5"
                      aria-hidden="true"
                    />
                  </div>
                  <input
                    id="search"
                    name="search"
                    class="block w-full bg-blue-700 bg-opacity-50 py-2 pl-10 pr-3 border border-transparent rounded-md leading-5 placeholder-blue-100 focus:outline-none focus:bg-white focus:ring-white focus:border-white focus:placeholder-gray-500 focus:text-gray-900 sm:text-sm"
                    placeholder="Search"
                    type="search"
                  />
                </div>
              </div>
            </div>
            <div class="flex lg:hidden">
              <!-- Mobile menu button -->
              <DisclosureButton
                class="p-2 rounded-md inline-flex items-center justify-center text-blue-200 hover:text-white hover:bg-blue-800 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
              >
                <span class="sr-only">Open main menu</span>
                <MenuIcon
                  v-if="!open"
                  class="block flex-shrink-0 h-6 w-6"
                  aria-hidden="true"
                />
                <XIcon
                  v-else
                  class="block flex-shrink-0 h-6 w-6"
                  aria-hidden="true"
                />
              </DisclosureButton>
            </div>
            <div class="hidden lg:block lg:ml-4">
              <div class="flex items-center">
                <button
                  class="flex-shrink-0 rounded-full p-1 text-blue-200 hover:bg-blue-800 hover:text-white focus:outline-none focus:bg-blue-900 focus:ring-2 focus:ring-offset-2 focus:ring-offset-blue-900 focus:ring-white"
                >
                  <span class="sr-only">View notifications</span>
                  <BellIcon class="h-6 w-6" aria-hidden="true" />
                </button>

                <!-- Profile dropdown -->
                <Menu as="div" class="relative flex-shrink-0 ml-4">
                  <div>
                    <MenuButton
                      class="rounded-full flex text-sm text-white focus:outline-none focus:bg-blue-900 focus:ring-2 focus:ring-offset-2 focus:ring-offset-blue-900 focus:ring-white"
                    >
                      <span class="sr-only">Open user menu</span>
                      <img
                        class="rounded-full h-8 w-8"
                        :src="user.imageUrl"
                        alt=""
                      />
                    </MenuButton>
                  </div>
                  <transition
                    enter-active-class="transition ease-out duration-100"
                    enter-from-class="transform opacity-0 scale-95"
                    enter-to-class="transform opacity-100 scale-100"
                    leave-active-class="transition ease-in duration-75"
                    leave-from-class="transform opacity-100 scale-100"
                    leave-to-class="transform opacity-0 scale-95"
                  >
                    <MenuItems
                      class="origin-top-right absolute right-0 mt-2 w-48 rounded-md shadow-lg py-1 bg-white ring-1 ring-black ring-opacity-5 focus:outline-none"
                    >
                      <MenuItem
                        v-for="item in userNavigation"
                        :key="item.name"
                        v-slot="{ active }"
                      >
                        <a
                          :href="item.href"
                          :class="[
                            active ? 'bg-gray-100' : '',
                            'block py-2 px-4 text-sm text-gray-700',
                          ]"
                          >{{ item.name }}</a
                        >
                      </MenuItem>
                    </MenuItems>
                  </transition>
                </Menu>
              </div>
            </div>
          </div>
        </div>

        <DisclosurePanel class="bg-blue-900 lg:hidden">
          <div class="pt-2 pb-3 px-2 space-y-1">
            <a
              v-for="item in navigation"
              :key="item.name"
              :href="item.href"
              :class="[
                item.current
                  ? 'bg-black bg-opacity-25'
                  : 'hover:bg-blue-800',
                'block rounded-md py-2 px-3 text-base font-medium text-white',
              ]"
              >{{ item.name }}</a
            >
          </div>
          <div class="pt-4 pb-3 border-t border-blue-800">
            <div class="flex items-center px-4">
              <div class="flex-shrink-0">
                <img
                  class="rounded-full h-10 w-10"
                  :src="user.imageUrl"
                  alt=""
                />
              </div>
              <div class="ml-3">
                <div class="text-base font-medium text-white">
                  {{ user.name }}
                </div>
                <div class="text-sm font-medium text-blue-200">
                  {{ user.email }}
                </div>
              </div>
              <button
                class="ml-auto flex-shrink-0 rounded-full p-1 text-blue-200 hover:bg-blue-800 hover:text-white focus:outline-none focus:bg-blue-900 focus:ring-2 focus:ring-offset-2 focus:ring-offset-blue-900 focus:ring-white"
              >
                <span class="sr-only">View notifications</span>
                <BellIcon class="h-6 w-6" aria-hidden="true" />
              </button>
            </div>
            <div class="mt-3 px-2">
              <a
                v-for="item in userNavigation"
                :key="item.name"
                :href="item.href"
                class="block rounded-md py-2 px-3 text-base font-medium text-blue-200 hover:text-white hover:bg-blue-800"
                >{{ item.name }}</a
              >
            </div>
          </div>
        </DisclosurePanel>
      </nav>
      <div
        :class="[
          open ? 'bottom-0' : 'inset-y-0',
          'absolute flex justify-center inset-x-0 left-1/2 transform -translate-x-1/2 w-full overflow-hidden lg:inset-y-0',
        ]"
        aria-hidden="true"
      >
        <div class="flex-grow bg-blue-900 bg-opacity-75" />
        <svg
          class="flex-shrink-0"
          width="1750"
          height="308"
          viewBox="0 0 1750 308"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            opacity=".75"
            d="M1465.84 308L16.816 0H1750v308h-284.16z"
            fill="#075985"
          />
          <path
            opacity=".75"
            d="M1733.19 0L284.161 308H0V0h1733.19z"
            fill="#0c4a6e"
          />
        </svg>
        <div class="flex-grow bg-blue-800 bg-opacity-75" />
      </div>
      <header class="relative py-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <h1 class="text-3xl font-bold text-white">Settings</h1>
        </div>
      </header>
    </Disclosure>

    <main class="relative -mt-32">
      <div class="max-w-screen-xl mx-auto pb-6 px-4 sm:px-6 lg:pb-16 lg:px-8">
        <div class="bg-white rounded-lg shadow overflow-hidden">
          <div
            class="divide-y divide-gray-200 lg:grid lg:grid-cols-12 lg:divide-y-0 lg:divide-x"
          >
            <aside class="py-6 lg:col-span-3">
              <nav class="space-y-1">
                <a
                  v-for="(item, i) in subNavigation" 
                   @click="go(item.href, i)"
                  :key="item.name"
                  
                  
                
                  :class="[
                    item.current
                      ? 'bg-blue-100 border-blue-500 text-blue-900 hover:bg-blue-50 hover:text-blue-700'
                      : 'border-transparent text-gray-900 hover:bg-gray-100 hover:text-gray-900',
                    'group border-l-4 px-3 py-2 flex items-center text-sm font-medium',
                  ]"
                  :aria-current="item.current ? 'page' : undefined"
                >
                  <component
                    :is="item.icon"
                    :class="[
                      item.current
                        ? 'text-teal-500 group-hover:text-teal-500'
                        : 'text-gray-400 group-hover:text-gray-500',
                      'flex-shrink-0 -ml-1 mr-3 h-6 w-6',
                    ]"
                    aria-hidden="true"
                  />
                  <span class="truncate">
                    {{ item.name }}
                  </span>
                </a>
              </nav>
            </aside>
            <slot></slot>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from "vue";
import {
  Disclosure,
  DisclosureButton,
  DisclosurePanel,
  Menu,
  MenuButton,
  MenuItem,
  MenuItems,
  Switch,
  SwitchDescription,
  SwitchGroup,
  SwitchLabel,
} from "@headlessui/vue";
import { SearchIcon } from "@heroicons/vue/solid";
import {
  BellIcon,
  CogIcon,
  CreditCardIcon,
  KeyIcon,
  MenuIcon,
  UserCircleIcon,
  ViewGridAddIcon,
  XIcon,
} from "@heroicons/vue/outline";

import { reactive } from "vue";
import { useRouter } from "vue-router";
import { useRoute } from "vue-router";

const user = {
  name: "Debbie Lewis",
  handle: "deblewis",
  email: "debbielewis@example.com",
  imageUrl:
    "https://images.unsplash.com/photo-1517365830460-955ce3ccd263?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=4&w=320&h=320&q=80",
};
const navigation = [
  { name: "Dashboard", href: "#", current: true },
  { name: "Jobs", href: "#", current: false },
  { name: "Applicants", href: "#", current: false },
  { name: "Company", href: "#", current: false },
];

const userNavigation = [
  { name: "Your Profile", href: "#" },
  { name: "Settings", href: "#" },
  { name: "Sign out", href: "#" },
];

export default {
  name: 'settings_1',
  components: {
    Disclosure,
    DisclosureButton,
    DisclosurePanel,
    Menu,
    MenuButton,
    MenuItem,
    MenuItems,
    Switch,
    SwitchDescription,
    SwitchGroup,
    SwitchLabel,
    BellIcon,
    MenuIcon,
    SearchIcon,
    XIcon,
    
  },
  setup(props, context) {
    const open = ref(false);
    const availableToHire = ref(true);
    const privateAccount = ref(false);
    const allowCommenting = ref(true);
    const allowMentions = ref(true);

    const router = useRouter();
    const sidebarOpen = ref(false);
    const route = useRoute();
    const subNavigation = ref ([
      {
        name: "Profile",
        href: "profile",
        icon: "UserCircleIcon",
        current: route.name.slice(0, 7) == "profile",
      },
      {
        name: "Account",
        href: "account",
        icon: "CogIcon",
        current: route.name.slice(0, 7) == "account",
      },
      {
        name: "Password",
        href: "password",
        icon: "KeyIcon",
        current: route.name.slice(0, 5) == "password",
      },
      {
        name: "Notifications",
        href: "notification",
        icon: "ViewGridAddIcon",
        current: route.name.slice(0, 9) == "notification",
      },
      {
        name: "Billing",
        href: "billing",
        icon: CreditCardIcon,
        current: route.name.slice(0, 8) == "billing",
      },
      {
        name: "Company",
        href: "company",
        icon: ViewGridAddIcon,
        current: route.name.slice(0, 6) == "Company",
      },
      {
        name: "Social",
        href: "social",
        icon: ViewGridAddIcon,
        current: route.name.slice(0, 6) == "Social",
      },
     
      
    ])

    const go = (route, i) => {
      router.push({ name: route });
      subNavigation.value.forEach((e) => {
        e.current = false;
      });
      subNavigation.value[i].current = true;
    }

    return {
      user,
      navigation,
      subNavigation,
      userNavigation,
      open,
      availableToHire,
      privateAccount,
      allowCommenting,
      allowMentions,
      go,
      sidebarOpen,
    }
  },
}
</script>