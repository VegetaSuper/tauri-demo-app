<script setup lang="ts">
import {
	getName,
	getVersion,
	getTauriVersion,
	hide,
	show,
} from '@tauri-apps/api/app';
import { onMounted, reactive } from 'vue';

const data = reactive({
	appName: '',
	appVersion: '',
	tauriVersion: '',
});

async function handleHide(time: number = 0) {
	await hide();
	if (time) {
		setTimeout(async () => {
			await show();
		}, time);
	}
}

onMounted(async () => {
	data.appName = await getName();
	data.appVersion = await getVersion();

	data.tauriVersion = await getTauriVersion();
});
</script>

<template>
	<ul class="flex flex-col gap-2">
		<li>应用名称: {{ data.appName }}</li>
		<li>应用版本: {{ data.appVersion }}</li>
		<li>Tauri版本: {{ data.tauriVersion }}</li>
		<li class="text-gray-400">--以下仅MacOS支持--</li>
		<li
			class="cursor-pointer"
			@click="handleHide()">
			隐藏程序
		</li>
		<li
			class="cursor-pointer"
			@click="handleHide(2000)">
			隐藏后2s显示程序
		</li>
	</ul>
</template>
