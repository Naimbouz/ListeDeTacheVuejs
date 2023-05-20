<!-- cette version ne consomme pas d'api rest -->
<template>
    <div class="tache-list">
        <input v-model="newtache" placeholder="Add a tache" class="tache-input">
        <button @click="addtache" class="add-button">
            <i class="fas fa-plus"></i> Add
        </button>
        <ul>
            <li v-for="(tache, index) in taches" :key="index" class="tache-item">
                <input type="checkbox" v-model="tache.completed" class="tache-checkbox">
                <input v-model="tache.title" :disabled="!tache.editing" class="tache-title"
                    :class="{ 'completed-tache': tache.completed }">
                <button @click="edittache(tache)" class="edit-button">
                    <i class="fas fa-pencil-alt"></i> Edit
                </button>
                <button @click="deletetache(index)" class="delete-button">
                    <i class="fas fa-trash-alt"></i> Delete
                </button>
                <button v-if="tache.editing" @click="savetache(tache)" class="save-button">
                    <i class="fas fa-save"></i> Save
                </button>
            </li>
        </ul>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            newtache: '',
            taches: [],
        };
    },
    methods: {
        addtache() {
            if (this.newtache.trim() !== '') {
                this.taches.push({ title: this.newtache, editing: false, completed: false });
                this.newtache = '';
            }
        },
        edittache(tache) {
            tache.editing = true;
        },
        savetache(tache) {
            tache.editing = false;
        },
        deletetache(index) {
            this.taches.splice(index, 1);
        },
    },
};
</script>
  
<style scoped>
.tache-list {
    max-width: 400px;
    margin: 0 auto;
}

.tache-input {
    margin-bottom: 10px;
    padding: 5px;
    width: 100%;
}

.add-button {
    display: block;
    margin-bottom: 10px;
    padding: 5px 10px;
}

.tache-item {
    display: flex;
    align-items: center;
    margin-bottom: 5px;
}

.tache-checkbox {
    margin-right: 10px;
}

.tache-title {
    flex-grow: 1;
    margin-right: 10px;
    padding: 5px;
}

.completed-tache {
    text-decoration: line-through;
}

.edit-button,
.delete-button,
.save-button {
    margin-left: 5px;
    padding: 5px 10px;
}

.fa {
    margin-right: 5px;
}
</style>
  

<!-- cette version est avec l'api -->
<!-- <template>
    <div class="taches-list">
        <input v-model="newtache" placeholder="Add a tache" class="tache-input">
        <button @click="addtache" class="add-button">
            <i class="fas fa-plus"></i> Add
        </button>
        <ul>
            <li v-for="(tache) in taches" :key="tache.id" class="tache-item">
                <input type="checkbox" v-model="tache.completed" class="tache-checkbox">
                <input v-model="tache.title" :disabled="!tache.editing" class="tache-title"
                    :class="{ 'completed-tache': tache.completed }">
                <button @click="edittache(tache)" class="edit-button">
                    <i class="fas fa-pencil-alt"></i> Edit
                </button>
                <button @click="deletetache(tache.id)" class="delete-button">
                    <i class="fas fa-trash-alt"></i> Delete
                </button>
                <button v-if="tache.editing" @click="savetache(tache)" class="save-button">
                    <i class="fas fa-save"></i> Save
                </button>
            </li>
        </ul>
        <div class="profile-info">
            <h3>{{ profile.name }}</h3>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    data() {
        return {
            newtache: '',
            taches: [],
            profile: {},
        };
    },
    mounted() {
        this.fetchTaches();
        this.fetchProfile();
    },
    methods: {
        async fetchTaches() {
            try {
                const response = await axios.get('http://localhost:3000/taches');
                this.taches = response.data.taches;
            } catch (error) {
                console.error(error);
            }
        },
        async fetchProfile() {
            try {
                const response = await axios.get('http://localhost:3000/taches');
                this.profile = response.data;
            } catch (error) {
                console.error(error);
            }
        },
        async addtache() {
            if (this.newtache.trim() !== '') {
                try {
                    const response = await axios.post('http://localhost:3000/taches', {
                        title: this.newtache,
                    });
                    this.taches.push(response.data);
                    this.newtache = '';
                } catch (error) {
                    console.error(error);
                }
            }
        },
        edittache(tache) {
            tache.editing = true;
        },
        async savetache(tache) {
            tache.editing = false;
            try {
                await axios.put(`http://localhost:3000/taches/${tache.id}`, tache);
            } catch (error) {
                console.error(error);
            }
        },
        async deletetache(id) {
            try {
                await axios.delete(`http://localhost:3000/taches/${id}`);
                this.taches = this.taches.filter((tache) => tache.id !== id);
            } catch (error) {
                console.error(error);
            }
        },
    },
};
</script>
<style scoped>
.tache-list {
    max-width: 400px;
    margin: 0 auto;
}

.tache-input {
    margin-bottom: 10px;
    padding: 5px;
    width: 100%;
}

.add-button {
    display: block;
    margin-bottom: 10px;
    padding: 5px 10px;
}

.tache-item {
    display: flex;
    align-items: center;
    margin-bottom: 5px;
}

.tache-checkbox {
    margin-right: 10px;
}

.tache-title {
    flex-grow: 1;
    margin-right: 10px;
    padding: 5px;
}

.completed-tache {
    text-decoration: line-through;
}

.edit-button,
.delete-button,
.save-button {
    margin-left: 5px;
    padding: 5px 10px;
}

.fa {
    margin-right: 5px;
}
</style>  -->