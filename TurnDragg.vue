<template>

    <div class="row row-cols-lg-12 g-10 droppable">
        <!--begin::Col-->
        <div class="col-6" v-for="category in categories" :key="category.cat" @drop="onDrop($event, category.cat)"
            @dragover.prevent @dragenter.prevent>
            <!--begin::Card-->
            <div class="card card-bordered">
                <!--begin::Card header-->
                <div class="card-header">
                    <div class="card-title">
                        <h3 class="card-label">{{ category.title }}</h3>
                    </div>
                </div>
                <!--end::Card header-->
                <div class="scroll px-5 scroll_turn">
                    <!--begin::Card body-->
                    <div class="card-body">
                        <!--begin::Row-->
                        <div v-for="item in Object.values(turns).filter(x => x.cat == category.cat)" :class="{turnsAdd:item.cat==1, turnsNotAdd:item.cat==0}"
                            @dragstart="onDragStart($event, item)" class="draggable" draggable="true">
                            <p>{{ item.title }}</p>
                        </div>

                        <!--end::Row-->
                    </div>
                    <!--end::Card body-->
                </div>
            </div>
            <!--end::Card-->
        </div>
        <!--end::Col-->
    </div>


</template>

<script lang="ts">

export default {
    name: 'App',
    components: {
    },
    props: {
        turns: null
    },

    data() {
        return {
            categories: [
                {
                    cat: 1,
                    title: 'Подключенные'
                },
                {
                    cat: 0,
                    title: 'Отключенные'
                }
            ],
        }
    },

    emits: {
        selectChange: null,
    },
    mounted() {
        this.loadTurns();

    },
    methods: {
        loadTurns() {
            this.$emit('selectChange');
        },

        onDragStart(e: DragEvent, item) {
            e.dataTransfer.dropEffect = 'move';
            e.dataTransfer.effectAllowed = 'move';
            e.dataTransfer.setData('itemUuid', item.uuid);
        },
        onDrop(e: DragEvent, cat) {
            const itemUuid = e.dataTransfer.getData('itemUuid');
            for (const i in this.turns) {
                if (this.turns[i].uuid == itemUuid) {
                    this.turns[i].cat = cat;

                };
            };

        },
    }

}
</script>
<style>
.draggable {
    margin-bottom: 3px;
    text-align: center;
    color: #5E6278;
    cursor: grab;
}

.draggable p {
    margin: 0;
    padding-top: 0.5rem;
   padding-bottom: 0.5rem;
}

.turnsAdd{
    border: 1px solid #50cd89;
    border-radius: 10px;
    border-style: dashed;


}
.turnsAdd:hover {
    background-color: #e8fff3;
    transition: all 0.5s;
}



.turnsNotAdd {
    border: 1px solid #f1416c;
    border-radius: 10px;
    border-style: dashed;

}

.turnsNotAdd:hover {
background-color: #fff5f8;
transition: all 0.5s;

}

.draggable:active {
    cursor: grabbing;
}
</style>




