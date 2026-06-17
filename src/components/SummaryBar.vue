<script setup lang="ts">
import { Palette, Users, Clock, Package, AlertTriangle, CheckCircle } from 'lucide-vue-next';
import type { Alert } from '../types';

interface Props {
  totalStyles: number;
  totalGroups: number;
  totalPeople: number;
  peopleInGroups: number;
  totalDuration: number;
  assignedDuration: number;
  totalMaterials: number;
  alerts: Alert[];
}

defineProps<Props>();

const formatDuration = (minutes: number) => {
  const hours = Math.floor(minutes / 60);
  const mins = minutes % 60;
  if (hours > 0) {
    return `${hours}小时${mins > 0 ? mins + '分' : ''}`;
  }
  return `${mins}分钟`;
};
</script>

<template>
  <div class="fixed bottom-0 left-0 right-0 bg-gradient-to-r from-gray-800 to-gray-900 text-white shadow-lg z-50">
    <div class="max-w-6xl mx-auto px-3 sm:px-4 py-2 sm:py-3">
      <div class="flex items-center justify-between flex-wrap gap-y-2">
        <div class="flex items-center gap-3 sm:gap-6 flex-wrap">
          <div class="flex items-center gap-1.5 sm:gap-2">
            <Palette class="w-4 h-4 sm:w-5 sm:h-5 text-green-400" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalStyles }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">样式</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Users class="w-4 h-4 sm:w-5 sm:h-5 text-blue-400" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalPeople }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">人</span>
              <span class="text-gray-500 mx-0.5 sm:mx-1">/</span>
              <span class="font-semibold" :class="peopleInGroups !== totalPeople ? 'text-amber-400' : 'text-blue-300'">{{ peopleInGroups }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">在组</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Clock class="w-4 h-4 sm:w-5 sm:h-5 text-amber-400" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold" :class="assignedDuration > totalDuration ? 'text-red-400' : 'text-amber-300'">
                {{ formatDuration(assignedDuration) }}
              </span>
              <span class="text-gray-500 mx-0.5 sm:mx-1">/</span>
              <span class="text-gray-400">{{ formatDuration(totalDuration) }}</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Package class="w-4 h-4 sm:w-5 sm:h-5 text-purple-400" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalMaterials }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">材料</span>
            </span>
          </div>
        </div>

        <div class="flex items-center gap-2 sm:gap-4">
          <div v-if="alerts.length > 0" class="flex items-center gap-1.5 sm:gap-2">
            <AlertTriangle class="w-4 h-4 sm:w-5 sm:h-5 text-amber-400" />
            <span class="text-xs sm:text-sm">
              <span class="text-red-400 font-semibold">
                {{ alerts.filter(a => a.severity === 'error').length }}
              </span>
              <span class="text-gray-400">错</span>
              <span class="text-gray-500 mx-0.5 sm:mx-1">·</span>
              <span class="text-amber-400 font-semibold">
                {{ alerts.filter(a => a.severity === 'warning').length }}
              </span>
              <span class="text-gray-400">警</span>
            </span>
          </div>
          <div v-else class="flex items-center gap-1.5 sm:gap-2 text-green-400">
            <CheckCircle class="w-4 h-4 sm:w-5 sm:h-5" />
            <span class="text-xs sm:text-sm font-medium">正常</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
