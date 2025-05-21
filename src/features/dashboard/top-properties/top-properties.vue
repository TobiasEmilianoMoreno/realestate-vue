<template>
    <div class="top-properties">
        <div class="top-properties__header">
            <h3 class="top-properties__title">Top Properties</h3>
            <a href="#" class="top-properties__action">
                See More
                <span class="top-properties__icon">→</span>
            </a>
        </div>
        <div class="error-message" v-if="error">
            {{ error }}
        </div>
        <div class="loading" v-if="isLoading">Cargando propiedades...</div>
        <ul v-show="!isLoading" class="top-properties__list">
            <li v-for="property in properties" :key="property.id" class="top-properties__item">
                <img :src="property.imageUrl" :alt="property.buildingName" class="top-properties__avatar" />
                <div class="top-properties__info">
                    <span class="top-properties__name">{{ property.buildingName }}</span>
                    <span class="top-properties__location">{{ property.direction }}</span>
                </div>
                <span :class="[
                    'top-properties__change',
                    property.pctChange > 0
                        ? 'top-properties__change--up'
                        : 'top-properties__change--down'
                ]">
                    {{ (property.pctChange > 0 ? '↑' : '↓') + Math.abs(property.pctChange) + '%' }}
                </span>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { TopProperty } from './top-property';
import { api } from '@/plugins/axios';

const properties = ref<TopProperty[]>([])
const isLoading = ref(false)
const error = ref<string | null>(null)

const fetchProperties = async () => {
    isLoading.value = true
    error.value = null
    try {
        console.log(localStorage.getItem('token'));
        
        const { data } = await api.get<TopProperty[]>('/buildings/overview', {
            headers: {
                'Authorization': `Bearer ${localStorage.getItem('token')}`
            }
        })
        properties.value = data
    } catch{
        error.value = 'No se pudieron cargar las propiedades.'
    } finally {
        isLoading.value = false
    }
}

onMounted(() => {
    fetchProperties()
})

</script>

<style scoped src="./top-properties.scss" />
