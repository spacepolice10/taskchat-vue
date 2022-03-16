<script setup>
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue";
import { DotsVerticalIcon, PencilIcon, TrashIcon } from "@heroicons/vue/solid";
import { ref } from "@vue/reactivity";
import { watch } from "vue";
import supabase from "../../../lib/db";
const props = defineProps(["id"]);
const emits = defineEmits(["cardDeleted"]);
let id = props.id;

async function deleteCard() {
  emits("cardDeleted");
  const {} = await supabase.from("cards").delete().eq("id", props.id);
}
</script>

<template>
  <Menu as="div" class="sticky">
    <MenuButton>
      <DotsVerticalIcon class="h-6 w-6 hover:text-blue-600" />
    </MenuButton>
    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="translate-y-1 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-1 opacity-0"
    >
      <MenuItems
        class="absolute z-10 -ml-20 origin-top-right rounded-xl bg-white p-4 shadow-xl"
      >
        <MenuItem disabled>
          <button
            trib="Disabled at the moment"
            class="flex w-full items-center gap-2 rounded-lg p-2 duration-200 after:absolute after:-left-[80px] after:mt-16 after:w-10/12 after:rounded-tl-md after:rounded-br-md after:bg-black after:px-2 after:text-sm after:text-white after:opacity-0 after:duration-200 after:content-[attr(trib)] hover:bg-slate-50 hover:after:opacity-80"
          >
            <PencilIcon class="h-4 w-4" />
            Edit
          </button>
        </MenuItem>
        <MenuItem>
          <button
            @click="deleteCard"
            class="flex w-full items-center gap-2 rounded-lg p-2 text-red-600 hover:bg-red-100"
          >
            <TrashIcon class="h-4 w-4" />
            Delete
          </button>
        </MenuItem>
      </MenuItems>
    </transition>
  </Menu>
</template>
