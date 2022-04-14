<template>
  <q-page class="row q-pa-md padding">
    <div class="col-12">
      <q-toggle
      v-model="dark"
      label="dark"
      @click="toogleDark"
    />
      <q-select
        v-model="data.typeOfBranch"
        :options="options"
        label="Type of branch"
      />
      <q-input
        clearable
        v-model="data.workItemId"
        type="number"
        label="Work item ID"
      />
      <q-input clearable v-model="data.workItemTitle" label="Work item title" />
      <q-input filled v-model="branchName" label="Create Branch">
        <template v-slot:append >
          <q-icon name="content_copy" @click="copy(branchName)" class="cursor-pointer"/>
        </template>
      </q-input>
      <q-select
        v-model="data.typeOfPush"
        :options="typesOfPush"
        label="Type of push"
      />
      <q-input filled v-model="pushBranch" label="Push Branch">
        <template v-slot:append >
          <q-icon name="content_copy" @click="copy(pushBranch)" class="cursor-pointer"/>
        </template>
      </q-input>
      <!-- <q-btn class="full-width" label="Save on store" @click="saveStore"/> -->
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from 'vue';
import { copyToClipboard, useQuasar } from 'quasar';

interface Values {
  typeOfBranch: string
  workItemId: number
  workItemTitle: string
  typeOfPush: string
}

// const BRANCH_NAME = 'branch_name'
export default defineComponent({
  name: 'PageIndex',
  setup() {
    const $q = useQuasar()

    const data = reactive({
      typeOfBranch: 'feature',
      workItemId: 3000,
      workItemTitle: 'description of work item',
      typeOfPush: 'upstream'
    } as Values);

    // const saveStore = () => {
    //   $q.localStorage.set(BRANCH_NAME, data)
    // }
  
    const getBranchName = () => {
      if (!data.typeOfBranch || !data.workItemId || !data.workItemTitle) return ''
      const { typeOfBranch, workItemId, workItemTitle } = data
      const cleanWorkItemTitle = workItemTitle ? String(workItemTitle).replace(/([a-z])([A-Z])/g, '$1-$2').replace(/\s+/g, '_').toLowerCase() : ''
      return `${String(typeOfBranch)}/${String(workItemId)}_${cleanWorkItemTitle}`;
    };

    const checkoutBranch = () => getBranchName() ? `git checkout -b ${getBranchName()}` : ''

    const getPushBranch = () => {
      if (!getBranchName()) return ''
      const type = data.typeOfPush === 'upstream' ? '--set-upstream' : '--force'
      return `git push ${type} origin ${getBranchName()}`
    };

    const toogleDark = () => $q.dark.toggle()


    const copy = (data: string) => copyToClipboard(data)
      .then(() => {
        alert('Copied to clipboard')

      })
      .catch(() => {
         alert('Error when copying')
      });

    const options = ['feature', 'bug']
    const typesOfPush = ['upstream', 'force']
    return {
      dark: computed(() => $q.dark.isActive),
      options,
      typesOfPush,
      data,
      branchName: computed(() => checkoutBranch()),
      pushBranch: computed(() => getPushBranch()),
      toogleDark,
      // saveStore,
      copy
    };
  },
});
</script>

<style scoped>
.padding {
  padding: 3rem;
}
</style>
