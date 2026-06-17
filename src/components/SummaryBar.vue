<script setup lang="ts">
import { Palette, Users, Clock, Package, AlertTriangle, CheckCircle } from 'lucide-vue-next';
import type { Alert } from '../types';

interface Props {
  totalStyles: number;
  totalGroups: number;
  totalPeople: number;
  totalActivityPeople: number;
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
    <div class="max-w-6xl mx-auto px-3 py-2 sm:px-4 sm:py-3">
      <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
        <div class="grid grid-cols-2 sm:flex sm:items-center gap-2 sm:gap-4 md:gap-6">
          <div class="flex items-center gap-1.5 sm:gap-2">
            <Palette class="w-4 h-4 sm:w-5 sm:h-5 text-green-400 flex-shrink-0" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalStyles }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">样式</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Users class="w-4 h-4 sm:w-5 sm:h-5 text-blue-400 flex-shrink-0" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalGroups }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">组</span>
              <span class="text-gray-500 mx-1 hidden sm:inline">·</span>
              <span 
                class="font-semibold hidden sm:inline"
                :class="totalActivityPeople > 0 && totalPeople > totalActivityPeople ? 'text-red-400' : ''"
              >{{ totalPeople }}</span>
              <span class="text-gray-400 ml-0.5 hidden sm:inline">人/</span>
              <span class="text-gray-300 hidden sm:inline">{{ totalActivityPeople || 0 }}</span>
              <span class="text-gray-400 ml-0.5 hidden sm:inline">总</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Clock class="w-4 h-4 sm:w-5 sm:h-5 text-amber-400 flex-shrink-0" />
            <span class="text-xs sm:text-sm">
              <span 
                class="font-semibold" 
                :class="assignedDuration > totalDuration ? 'text-red-400' : 'text-amber-300'"
              >
                {{ formatDuration(assignedDuration) }}
              </span>
              <span class="text-gray-500 mx-0.5 sm:mx-1">/</span>
              <span class="text-gray-400 hidden sm:inline">{{ formatDuration(totalDuration) }}</span>
            </span>
          </div>

          <div class="flex items-center gap-1.5 sm:gap-2">
            <Package class="w-4 h-4 sm:w-5 sm:h-5 text-purple-400 flex-shrink-0" />
            <span class="text-xs sm:text-sm">
              <span class="font-semibold text-base sm:text-lg">{{ totalMaterials }}</span>
              <span class="text-gray-400 ml-0.5 sm:ml-1">材料</span>
            </span>
          </div>
        </div>

        <div class="flex items-center justify-center sm:justify-end gap-2 sm:gap-4">
          <div v-if="alerts.length > 0" class="flex items-center gap-1.5 sm:gap-2">
            <AlertTriangle class="w-4 h-4 sm:w-5 sm:h-5 text-amber-400 flex-shrink-0" />
            <span class="text-xs sm:text-sm">
              <span class="text-red-400 font-semibold">
                {{ alerts.filter(a => a.severity === 'error').length }}
              </span>
              <span class="text-gray-400">错</span>
              <span class="text-gray-500 mx-0.5 sm:mx-1">·</span>
              <span class="text-amber-400 font-semibold">
                {{ alerts.filter(a => a.severity === 'warning').length }}
              </span>
              <span class="text-gray-400 hidden sm:inline">个警告</span>
              <span class="text-gray-400 sm:hidden">警</span>
            </span>
          </div>
          <div v-else class="flex items-center gap-1.5 sm:gap-2 text-green-400">
            <CheckCircle class="w-4 h-4 sm:w-5 sm:h-5 flex-shrink-0" />
            <span class="text-xs sm:text-sm font-medium">一切正常</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
