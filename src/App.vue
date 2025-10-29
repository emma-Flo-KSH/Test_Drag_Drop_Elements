<template>
    <div class="max-w-6xl mx-auto">

        <h1 class="text-3xl font-bold text-center text-gray-800 mb-8">Demo Drag & Link (Vue 3)</h1>

        <div class="grid grid-cols-3 gap-8">

            <!-- Columna Izquierda (Fuentes) -->
            <div class="bg-gray-200 p-4 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-gray-800">Fuentes (Arrastra desde aquí)</h3>
                <div class="space-y-3">
                    <div
                        v-for="item in fuentes"
                        :key="item.id"
                        draggable="true"
                        @dragstart="iniciarDrag(item)"
                        @dragend="finalizarDrag"
                        class="p-4 mb-3 rounded-lg shadow bg-white cursor-move hover:bg-gray-50 select-none"
                        :class="{ 'opacity-50': itemArrastrado && itemArrastrado.id === item.id }"
                    >
                        {{ item.nombre }}
                    </div>
                </div>
            </div>

            <!-- Columna Derecha (Objetivos) -->
            <div class="bg-gray-200 p-4 rounded-lg shadow-md">
                <h3 class="text-xl font-bold mb-4 text-gray-800">Objetivos (Suelta aquí)</h3>
                <div class="space-y-3">
                    <div
                        v-for="item in objetivos"
                        :key="item.id"
                        @dragover.prevent
                        @dragenter.prevent="entrarZonaDrop(item)"
                        @dragleave="salirZonaDrop(item)"
                        @drop="soltarSobre(item)"
                        class="p-4 mb-3 rounded-lg shadow bg-white border-2 border-dashed border-gray-400 transition-all duration-150"
                        :class="{ 'bg-blue-100 border-solid border-blue-500 ring-2 ring-blue-500 scale-105': item.dragOver }"
                    >
                        {{ item.nombre }}
                    </div>
                </div>
            </div>

            <!-- Columna de Mapeo (Resultados) -->
            <div class="bg-green-100 p-4 rounded-lg shadow-inner col-span-1">
                <h3 class="text-xl font-bold mb-4 text-green-800">Indexación (Mapeo)</h3>

                <div v-if="Object.keys(mapeo).length === 0" class="text-gray-600">
                    Arrastra un elemento de la izquierda sobre uno de la derecha para vincularlos.
                </div>

                <div v-for="(valor, clave) in mapeo" :key="clave" class="font-mono text-green-900 bg-green-200 p-2 rounded mb-2">
                    <span class="font-bold">{{ clave }}</span>
                    <span> → </span>
                    <span>{{ valor }}</span>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            fuentes: [
                { id: 'f1', nombre: 'Usuario: Ana' },
                { id: 'f2', nombre: 'Usuario: Luis' },
                { id: 'f3', nombre: 'Usuario: Pedro' }
            ],
            objetivos: [
                { id: 'o1', nombre: 'Rol: Admin', dragOver: false },
                { id: 'o2', nombre: 'Rol: Editor', dragOver: false },
                { id: 'o3', nombre: 'Rol: Lector', dragOver: false }
            ],
            itemArrastrado: null,
            mapeo: {}
        };
    },
    methods: {
        iniciarDrag(item) {
            this.itemArrastrado = item;
        },
        soltarSobre(objetivo) {
            if (this.itemArrastrado) {
                this.mapeo[this.itemArrastrado.nombre] = objetivo.nombre;
                objetivo.dragOver = false;
            }
        },
        finalizarDrag() {
            this.itemArrastrado = null;
            this.objetivos.forEach(o => (o.dragOver = false));
        },
        entrarZonaDrop(objetivo) {
            if (this.itemArrastrado) objetivo.dragOver = true;
        },
        salirZonaDrop(objetivo) {
            objetivo.dragOver = false;
        }
    }
};
</script>

