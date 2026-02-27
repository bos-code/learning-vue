<script setup lang="ts">
import InputText from 'primevue/inputtext';
import Card from 'primevue/card';
import Dialog from 'primevue/dialog';
import { ref } from 'vue';
import { tasks } from './data.vue';

const goals = ref('');
const goalsList = ref<string[]>(tasks.map(task => task.title));
const showAddedPopup = ref(false);
const showRemovePopup = ref(false);
const showEditPopup = ref(false);
const goalToRemove = ref<number | null>(null);
const goalToEdit = ref<number | null>(null);
const editedGoalText = ref('');

function addGoal() {
  if (goals.value.trim() !== '') {
    goalsList.value.push(goals.value);
    goals.value = '';
    showAddedPopup.value = true;
  }
}

function removeGoal(index: number) {
  goalsList.value.splice(index, 1);
}

function askToRemove(index: number) {
  goalToRemove.value = index;
  showRemovePopup.value = true;
}

function confirmRemove() {
  if (goalToRemove.value !== null) {
    removeGoal(goalToRemove.value);
  }
  showRemovePopup.value = false;
  goalToRemove.value = null;
}

function editGoals(index: number) {
  goalToEdit.value = index;
  editedGoalText.value = goalsList.value[index];
  showEditPopup.value = true;
}

function saveEditedGoal() {
  if (goalToEdit.value !== null && editedGoalText.value.trim() !== '') {
    goalsList.value[goalToEdit.value] = editedGoalText.value.trim();
    showEditPopup.value = false;
    goalToEdit.value = null;
    editedGoalText.value = '';
  }
}
</script>

<template>
  <div class="min-h-screen bg-slate-950 text-slate-100">
    <div class="mx-auto max-w-6xl px-4 py-6 md:px-8 md:py-8">
      <nav class="mb-8 flex flex-wrap items-center justify-between gap-4 rounded-2xl border border-cyan-500/25 bg-slate-900/80 px-5 py-4 backdrop-blur">
        <div class="flex items-center gap-3">
          <div class="grid h-10 w-10 place-items-center rounded-xl bg-cyan-400 text-slate-900">
            <i class="pi pi-check-square text-lg"></i>
          </div>
          <div>
            <p class="text-lg font-bold tracking-wide">GoalTracker</p>
            <p class="text-xs text-slate-400">Personal productivity board</p>
          </div>
        </div>
        <ul class="flex items-center gap-2 text-sm">
          <li><a href="#" class="rounded-full bg-cyan-400 px-4 py-2 font-semibold text-slate-900">Dashboard</a></li>
          <li><a href="#" class="rounded-full px-4 py-2 text-slate-300 hover:bg-slate-800">Tasks</a></li>
          <li><a href="#" class="rounded-full px-4 py-2 text-slate-300 hover:bg-slate-800">Progress</a></li>
        </ul>
      </nav>

      <div class="grid gap-4 md:grid-cols-3">
        <div class="rounded-2xl border border-cyan-500/30 bg-gradient-to-br from-cyan-500/25 to-blue-500/10 p-5">
          <p class="text-xs uppercase tracking-widest text-cyan-200/80">Total Goals</p>
          <p class="mt-2 text-4xl font-extrabold">{{ goalsList.length }}</p>
        </div>
        <div class="rounded-2xl border border-fuchsia-400/30 bg-gradient-to-br from-fuchsia-500/20 to-purple-500/10 p-5">
          <p class="text-xs uppercase tracking-widest text-fuchsia-200/80">In Focus</p>
          <p class="mt-2 text-4xl font-extrabold">{{ goalsList.length > 0 ? Math.ceil(goalsList.length / 2) : 0 }}</p>
        </div>
        <div class="rounded-2xl border border-emerald-400/30 bg-gradient-to-br from-emerald-500/20 to-teal-500/10 p-5">
          <p class="text-xs uppercase tracking-widest text-emerald-200/80">Done Today</p>
          <p class="mt-2 text-4xl font-extrabold">{{ goalsList.length > 2 ? 2 : goalsList.length }}</p>
        </div>
      </div>

      <form @submit.prevent="addGoal" class="mt-6">
        <div class="rounded-2xl border border-slate-700 bg-slate-900/70 p-5">
          <p class="text-2xl font-bold">Plan your next move</p>
          <p class="mt-1 text-sm text-slate-400">Add goals, preview tasks, and keep momentum.</p>
          <div class="mt-4 flex flex-col gap-3 md:flex-row">
            <InputText class="w-full md:flex-1" placeholder="Enter your goal" v-model="goals" />
            <button type="submit" class="rounded-xl bg-cyan-400 px-5 py-2 font-semibold text-slate-900">
              <i class="pi pi-plus mr-2"></i>
              Add Goal
            </button>
          </div>
        </div>

        <section class="mt-4 rounded-2xl border border-slate-700 bg-slate-900/70 p-4">
          <div class="mb-3 flex items-center justify-between">
            <p class="font-semibold">Filters</p>
            <button type="button" class="text-sm text-cyan-300 hover:text-cyan-200">Reset</button>
          </div>
          <div class="grid grid-cols-1 gap-3 md:grid-cols-4">
            <InputText placeholder="Search tasks..." class="md:col-span-2 w-full" />
            <select class="w-full rounded-xl border border-slate-600 bg-slate-800 px-3 py-2 text-sm">
              <option>Status: All</option>
              <option>Todo</option>
              <option>In Progress</option>
              <option>Blocked</option>
              <option>Completed</option>
            </select>
            <select class="w-full rounded-xl border border-slate-600 bg-slate-800 px-3 py-2 text-sm">
              <option>Priority: All</option>
              <option>High</option>
              <option>Medium</option>
              <option>Low</option>
            </select>
          </div>
          <div class="mt-3 flex flex-wrap gap-2">
            <button type="button" class="rounded-full bg-cyan-400 px-3 py-1 text-xs font-semibold text-slate-900">All</button>
            <button type="button" class="rounded-full border border-slate-600 px-3 py-1 text-xs text-slate-300">UI</button>
            <button type="button" class="rounded-full border border-slate-600 px-3 py-1 text-xs text-slate-300">Bug</button>
            <button type="button" class="rounded-full border border-slate-600 px-3 py-1 text-xs text-slate-300">Auth</button>
            <button type="button" class="rounded-full border border-slate-600 px-3 py-1 text-xs text-slate-300">Performance</button>
          </div>
        </section>

        <Card class="mt-4 rounded-2xl border border-slate-700 bg-slate-900/70 text-slate-100">
          <template #content>
            <div class="mb-4 flex items-center justify-between">
              <p class="text-xl font-bold">Task List</p>
              <span class="rounded-full bg-slate-800 px-3 py-1 text-xs text-slate-300">{{ goalsList.length }} items</span>
            </div>
            <ol class="task-scrollbar max-h-[420px] space-y-3 overflow-y-auto pr-2">
              <li v-for="(goal, index) in goalsList" :key="goal"
                class="rounded-xl border border-slate-700 bg-slate-800/80 p-3 shadow-md">
                <div class="flex items-center justify-between gap-3">
                  <p class="text-base font-medium capitalize text-slate-100">{{ goal }}</p>
                  <div class="flex items-center gap-2">
                    <button type="button" @click="askToRemove(index)"
                      class="grid h-9 w-9 place-items-center rounded-lg bg-rose-500/90 text-white transition hover:bg-rose-500"
                      aria-label="Delete goal" title="Delete goal">
                      <i class="pi pi-trash"></i>
                    </button>
                    <button type="button" @click="editGoals(index)"
                      class="grid h-9 w-9 place-items-center rounded-lg bg-emerald-500/90 text-white transition hover:bg-emerald-500"
                      aria-label="Edit goal" title="Edit goal">
                      <i class="pi pi-pencil"></i>
                    </button>
                  </div>
                </div>
              </li>
            </ol>
          </template>
        </Card>

        <Dialog v-model:visible="showAddedPopup" modal header="Success" :style="{ width: '25rem' }">
          <p>Your goal was added.</p>
          <div class="mt-4 flex justify-end">
            <button type="button" class="rounded bg-cyan-500 px-3 py-2 text-white" @click="showAddedPopup = false">
              Close
            </button>
          </div>
        </Dialog>

        <Dialog v-model:visible="showRemovePopup" modal header="Confirm Remove" :style="{ width: '25rem' }">
          <p>Do you want to remove this goal?</p>
          <div class="mt-4 flex justify-end gap-2">
            <button type="button" class="rounded bg-slate-500 px-3 py-2 text-white" @click="showRemovePopup = false">
              Cancel
            </button>
            <button type="button" class="rounded bg-rose-500 px-3 py-2 text-white" @click="confirmRemove">
              Remove
            </button>
          </div>
        </Dialog>

        <Dialog v-model:visible="showEditPopup" modal header="Edit Goal" :style="{ width: '30rem' }">
          <InputText class="w-full" v-model="editedGoalText" placeholder="Edit goal" />
          <div class="mt-4 flex justify-end gap-2">
            <button type="button" class="rounded bg-slate-500 px-3 py-2 text-white" @click="showEditPopup = false">
              Cancel
            </button>
            <button type="button" class="rounded bg-emerald-500 px-3 py-2 text-white" @click="saveEditedGoal">
              Save
            </button>
          </div>
        </Dialog>
      </form>
    </div>
  </div>
</template>
