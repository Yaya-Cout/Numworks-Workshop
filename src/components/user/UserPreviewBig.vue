<template>
  <v-card>
    <v-card-title>
      <AvatarView :username="username" />
      <span class="ml-2">{{ username }}</span>
    </v-card-title>
    <v-card-text>
      <v-list>
        <v-list-item v-if="groups !== ''">
          <v-list-item-title>{{ $t('user.groups') }}</v-list-item-title>
          <v-skeleton-loader
            :loading="groups === 'none'"
            type="text"
            width="100"
          >
            <v-list-item-subtitle>{{ groups }}</v-list-item-subtitle>
          </v-skeleton-loader>
        </v-list-item>
      </v-list>
    </v-card-text>
  </v-card>
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { User } from '../../types';
import AvatarView from '../AvatarView.vue';

const props = defineProps({
  user: {
    type: Object as () => User,
    required: true,
  },
});

const groups = ref('');
const username = ref('');

watchEffect(async () => {
  username.value = await props.user?.username;
  const groupsAwaited = await props.user?.groups;
  let groupsString = '';
  for (const group of groupsAwaited || []) {
    const result = await group.name;
    groupsString += result + ', ';
  }
  groupsString = groupsString.slice(0, -2);
  groups.value = groupsString;
});
</script>

<style scoped></style>
