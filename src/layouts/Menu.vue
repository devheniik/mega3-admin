<template>
  <div class="h-screen flex overflow-hidden bg-white">
    <TransitionRoot as="template" :show="sidebarOpen">
      <Dialog as="div" static class="fixed inset-0 flex z-40 lg:hidden" @close="sidebarOpen = false"
        :open="sidebarOpen">
        <TransitionChild as="template" enter="transition-opacity ease-linear duration-300" enter-from="opacity-0"
          enter-to="opacity-100" leave="transition-opacity ease-linear duration-300" leave-from="opacity-100"
          leave-to="opacity-0">
          <DialogOverlay class="fixed inset-0 bg-gray-600 bg-opacity-75" />
        </TransitionChild>
        <TransitionChild as="template" enter="transition ease-in-out duration-300 transform"
          enter-from="-translate-x-full" enter-to="translate-x-0" leave="transition ease-in-out duration-300 transform"
          leave-from="translate-x-0" leave-to="-translate-x-full">
          <div class="relative flex-1 flex flex-col max-w-xs w-full pt-5 pb-4 bg-white">
            <TransitionChild as="template" enter="ease-in-out duration-300" enter-from="opacity-0"
              enter-to="opacity-100" leave="ease-in-out duration-300" leave-from="opacity-100" leave-to="opacity-0">
              <div class="absolute top-0 right-0 -mr-12 pt-2">
                <button
                  class="ml-1 flex items-center justify-center h-10 w-10 rounded-full focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
                  @click="sidebarOpen = false">
                  <span class="sr-only">Close sidebar</span>
                  <XIcon class="h-6 w-6 text-white" aria-hidden="true" />
                </button>
              </div>
            </TransitionChild>
            <div class="flex-shrink-0 flex items-center px-4">
              <img class="h-8 w-auto" src="img/logo_header.png" alt="Workflow" />
            </div>
            <div class="mt-5 flex-1 h-0 overflow-y-auto">
              <nav class="px-2">
                <div class="space-y-1">
                  <div v-for="(item, i) in navigation" :key="item.name">
                <a  @click="go(item.href, i)" :class="[
                    item.current
                      ? 'bg-gray-200 text-gray-900'
                      : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50',
                    'group flex items-center px-2 py-2 text-sm font-medium rounded-md',
                  ]" :aria-current="item.current ? 'page' : undefined">
                  <component :is="item.icon" :class="[
                      item.current
                        ? 'text-gray-500'
                        : 'text-gray-400 group-hover:text-gray-500',
                      'mr-3 h-6 w-6',
                    ]" aria-hidden="true" />
                  {{ item.name }}
                </a>
                <transition  enter-active-class="opacity-0 -translate-y-6 h-0" leave-active-class="opacity-0 -translate-y-6" >
                <div v-show="item.current" class="transform duration-300 ease-out">
                  <a v-for="(subItem, index) in item.child" :key="index" @click="go(subItem.href, i)" :class="[ false   ? 'bg-gray-200 text-gray-900'   : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50',  'group flex items-center pl-4 pr-2 py-2 text-sm font-medium rounded-md',  ]">
                  <component :is="subItem.icon" :class="[  false ? 'text-gray-500'   : 'text-gray-400 group-hover:text-gray-500', 'mr-3 h-6 w-6',   ]" aria-hidden="true" />
                  {{ subItem.name }}
                </a>
                </div>
                </transition>
              </div>
                </div>
                <div class="mt-8">
                  <h3 class="px-3 text-xs font-semibold text-gray-500 uppercase tracking-wider" id="teams-headline">
                    Teams
                  </h3>
                  <div class="mt-1 space-y-1" role="group" aria-labelledby="teams-headline">
                    <a v-for="team in teams" :key="team.name" :href="team.href"
                      class="group flex items-center px-3 py-2 text-base leading-5 font-medium text-gray-600 rounded-md hover:text-gray-900 hover:bg-gray-50">
                      <span :class="[
                          team.bgColorClass,
                          'w-2.5 h-2.5 mr-4 rounded-full',
                        ]" aria-hidden="true" />
                      <span class="truncate">
                        {{ team.name }}
                      </span>
                    </a>
                  </div>
                </div>
              </nav>
            </div>
          </div>
        </TransitionChild>
        <div class="flex-shrink-0 w-14" aria-hidden="true">
          <!-- Dummy element to force sidebar to shrink to fit close icon -->
        </div>
      </Dialog>
    </TransitionRoot>

    <!-- Static sidebar for desktop -->
    <div class="hidden lg:flex lg:flex-shrink-0">
      <div class="flex flex-col w-64 border-r border-gray-200 pt-5 pb-4 bg-gray-100">
        <div class="flex items-center flex-shrink-0 px-6">
          <img class="h-8 w-auto" src="img/logo_header.png" alt="Workflow" />
        </div>
        <!-- Sidebar component, swap this element with another sidebar if you like -->
        <div class="h-0 flex-1 flex flex-col overflow-y-auto scroll-block">
          <!-- User account dropdown -->
          <Menu as="div" class="px-3 mt-6 relative inline-block text-left">
            <div>
              <MenuButton
                class="group w-full bg-gray-100 rounded-md px-3.5 py-2 text-sm text-left font-medium text-gray-700 hover:bg-gray-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-gray-100 focus:ring-purple-500">
                <span class="flex w-full justify-between items-center">
                  <span class="flex min-w-0 items-center justify-between space-x-3">
                    <img class="w-10 h-10 bg-gray-300 rounded-full flex-shrink-0"
                      src="https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/12.03bf9466.png"
                      alt="" />
                    <span class="flex-1 flex flex-col min-w-0">
                      <span class="text-gray-900 text-sm font-medium truncate">Водим Рогинский</span>
                      <span class="text-gray-500 text-sm truncate">@roginskiy</span>
                    </span>
                  </span>
                  <SelectorIcon class="flex-shrink-0 h-5 w-5 text-gray-400 group-hover:text-gray-500"
                    aria-hidden="true" />
                </span>
              </MenuButton>
            </div>
            <transition enter-active-class="transition ease-out duration-100"
              enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
              leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100"
              leave-to-class="transform opacity-0 scale-95">
              <MenuItems
                class="z-10 mx-3 origin-top absolute right-0 left-0 mt-1 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 divide-y divide-gray-200 focus:outline-none">
                <div class="py-1">
                  <MenuItem v-slot="{ active }" @click="$router.push({ name: 'profile' })">
                  <a :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">View profile</a>
                  </MenuItem>
                  <MenuItem v-slot="{ active }">
                  <a href="#" :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">Settings</a>
                  </MenuItem>
                  <MenuItem v-slot="{ active }">
                  <a href="#" :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">Notifications</a>
                  </MenuItem>
                </div>
                <div class="py-1">
                  <MenuItem v-slot="{ active }">
                  <a href="#" :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">Get desktop app</a>
                  </MenuItem>
                  <MenuItem v-slot="{ active }">
                  <a href="#" :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">Support</a>
                  </MenuItem>
                </div>
                <div class="py-1">
                  <MenuItem v-slot="{ active }">
                  <a href="#" :class="[
                        active ? 'bg-gray-100 text-gray-900' : 'text-gray-700',
                        'block px-4 py-2 text-sm',
                      ]">Logout</a>
                  </MenuItem>
                </div>
              </MenuItems>
            </transition>
          </Menu>
          <!-- Sidebar Search -->
          <div class="px-3 mt-5">
            <label for="search" class="sr-only">Search</label>
            <div class="mt-1 relative rounded-md shadow-sm">
              <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none" aria-hidden="true">
                <SearchIcon class="mr-3 h-4 w-4 text-gray-400" aria-hidden="true" />
              </div>
              <input type="text" name="search" id="search"
                class="focus:ring-indigo-500 focus:border-indigo-500 block w-full pl-9 sm:text-sm border-gray-300 rounded-md"
                placeholder="Поиск" />
            </div>
          </div>
          <!-- Navigation -->
          <nav class="px-3 mt-6">
            <div class="space-y-1">
              <div v-for="(item, i) in navigation" :key="item.name">
                <a  @click="go(item.href, i)" :class="[
                    item.current
                      ? 'bg-gray-200 text-gray-900'
                      : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50',
                    'group flex items-center px-2 py-2 text-sm font-medium rounded-md',
                  ]" :aria-current="item.current ? 'page' : undefined">
                  <component :is="item.icon" :class="[
                      item.current
                        ? 'text-gray-500'
                        : 'text-gray-400 group-hover:text-gray-500',
                      'mr-3 h-6 w-6',
                    ]" aria-hidden="true" />
                  {{ item.name }}
                </a>
                <transition enter-active-class="opacity-0 -translate-y-6 h-0" leave-active-class="opacity-0 -translate-y-6">
                <div v-show="item.current" class="transform duration-300 ease-out">
                  <a v-for="(subItem, index) in item.child" :key="index" @click="go(subItem.href, i)" :class="[ false   ? 'bg-gray-200 text-gray-900'   : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50',  'group flex items-center pl-4 pr-2 py-2 text-sm font-medium rounded-md',  ]">
                  <component :is="subItem.icon" :class="[  false ? 'text-gray-500'   : 'text-gray-400 group-hover:text-gray-500', 'mr-3 h-6 w-6',   ]" aria-hidden="true" />
                  {{ subItem.name }}
                </a>
                </div>
                </transition>
              </div>
            </div>
            <div class="mt-8">
              <!-- Secondary navigation -->
              <h3 class="px-3 text-xs font-semibold text-gray-500 uppercase tracking-wider" id="teams-headline">
                Shops
              </h3>
              <div class="mt-1 space-y-1" role="group" aria-labelledby="teams-headline">
                <a v-for="shop in shops" :key="shop.name" :href="shop.href"
                  class="group flex items-center px-3 py-2 text-sm font-medium text-gray-700 rounded-md hover:text-gray-900 hover:bg-gray-50">
                  <span :class="[
                      shop.bgColorClass,
                      'w-2.5 h-2.5 mr-4 rounded-full',
                    ]" aria-hidden="true" />
                  <span class="truncate">
                    {{ shop.name }}
                  </span>
                </a>
              </div>
            </div>
          </nav>
        </div>
      </div>
    </div>
    <!-- Main column -->
    <div class="flex flex-col w-0 flex-1 overflow-hidden">
      <!-- Search header -->
      <div class="relative z-10 flex-shrink-0 flex h-16 bg-white border-b border-gray-200 lg:hidden">
        <button
          class="px-4 border-r border-gray-200 text-gray-500 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-purple-500 lg:hidden"
          @click="sidebarOpen = true">
          <span class="sr-only">Open sidebar</span>
          <MenuAlt1Icon class="h-6 w-6" aria-hidden="true" />
        </button>
        <div class="flex-1 flex justify-between px-4 sm:px-6 lg:px-8">
          <div class="flex-1 flex">
            <form class="w-full flex md:ml-0" action="#" method="GET">
              <label for="search_field" class="sr-only">Search</label>
              <div class="relative w-full text-gray-400 focus-within:text-gray-600">
                <div class="absolute inset-y-0 left-0 flex items-center pointer-events-none">
                  <SearchIcon class="h-5 w-5" aria-hidden="true" />
                </div>
                <input id="search_field" name="search_field"
                  class="block w-full h-full pl-8 pr-3 py-2 border-transparent text-gray-900 placeholder-gray-500 focus:outline-none focus:ring-0 focus:border-transparent focus:placeholder-gray-400 sm:text-sm"
                  placeholder="Главная" type="search" />
              </div>
            </form>
          </div>
          <div class="flex items-center">
            <!-- Profile dropdown -->
            <Menu as="div" class="ml-3 relative">
              <div>
                <MenuButton
                  class="max-w-xs bg-white flex items-center text-sm rounded-full focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-500">
                  <span class="sr-only">Open user menu</span>
                  <img class="h-8 w-8 rounded-full"
                    src="https://images.unsplash.com/photo-1502685104226-ee32379fefbe?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
                    alt="" />
                </MenuButton>
              </div>
              <transition enter-active-class="transition ease-out duration-100"
                enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
                leave-active-class="transition ease-in duration-75" leave-from-class="transform opacity-100 scale-100"
                leave-to-class="transform opacity-0 scale-95">
                <MenuItems
                  class="origin-top-right absolute right-0 mt-2 w-48 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 divide-y divide-gray-200 focus:outline-none">
                  <div class="py-1">
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">View profile</a>
                    </MenuItem>
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">Settings</a>
                    </MenuItem>
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">Notifications</a>
                    </MenuItem>
                  </div>
                  <div class="py-1">
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">Support</a>
                    </MenuItem>
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">Support</a>
                    </MenuItem>
                  </div>
                  <div class="py-1">
                    <MenuItem v-slot="{ active }">
                    <a href="#" :class="[
                          active
                            ? 'bg-gray-100 text-gray-900'
                            : 'text-gray-700',
                          'block px-4 py-2 text-sm',
                        ]">Logout</a>
                    </MenuItem>
                  </div>
                </MenuItems>
              </transition>
            </Menu>
          </div>
        </div>
      </div>
      <main class="flex-1 relative z-0 overflow-y-auto focus:outline-none bg-gray-200">
        <!-- Page title & actions -->
        <slot></slot>
      </main>
    </div>
  </div>
</template>

<script>
  import {  ref } from "vue";
  import { useRouter  } from "vue-router";
  import {useRoute } from "vue-router";

  const shops = [{
      name: "Mega3",
      href: "#",
      bgColorClass: "bg-green-500"
    },
    {
      name: "Prom",
      href: "#",
      bgColorClass: "bg-green-500"
    },
    {
      name: "Instagram",
      href: "#",
      bgColorClass: "bg-green-500"
    },
    {
      name: "OLX",
      href: "#",
      bgColorClass: "bg-green-500"
    },
    {
      name: "Rozetka",
      href: "#",
      bgColorClass: "bg-red-500"
    },
  ];
  

  export default {
    name: 'MenuLayout',
    setup(props, context) {
      const router = useRouter();
      const sidebarOpen = ref(false);
      const route = useRoute();
      const navigation = ref([{
          name: "Главная",
          href: "home",
          icon: "HomeIcon",
          current: route.name.slice(0, 4) == "home",
        },
        {
          name: "Заказы",
          href: "orders",
          icon: "ShoppingCartIcon",
          current: route.name.slice(0, 5) == "order",
        },
        {
          name: "Товары",
          href: "products",
          icon: "TagIcon",
          current: route.name.slice(0, 7) == "product",
          child: [
            {
              name: 'Переводы',
              icon: 'ClipboardCopyIcon',
              href: "products",
            },
            {
              name: 'Склад',
              icon: 'InboxInIcon',
              href: "products",
            },
            {
              name: 'Колекции',
              icon: 'SunIcon',
              href: "product-collections",
            },
          ]
        },
        {
          name: "Клиенты",
          href: "#",
          icon: "UserIcon",
          current: false
        },
        {
          name: "Аналитика",
          href: "#",
          icon: "ChartBarIcon",
          current: false
        },
      ]);

      const go = (route, i) => {
        router.push({
          name: route
        });
        navigation.value.forEach((e) => {
          e.current = false;
        });
        navigation.value[i].current = true;
      };

      return {
        navigation,
        shops, go, sidebarOpen
      };
    },
  };
</script>

<style scoped>
/* хром, сафари */
.scroll-block::-webkit-scrollbar { width: 0; }

/* ie 10+ */
.scroll-block { -ms-overflow-style: none; }

/* фф (свойство больше не работает, других способов тоже нет)*/
.scroll-block { overflow: -moz-scrollbars-none; }
</style>