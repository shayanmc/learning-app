<template>
    <base-card>
        <base-button @click="setSelectedTab('store-resources' )" :mode="storedResButtonMode">Stored Resources</base-button>
        <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode">Add Resources</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>


<script >

import StoreResources from "./StoreResources.vue";
import AddResource from "./AddResource.vue";

export default ({
    components : {
        StoreResources,
        AddResource
    },
    data() {
        return {
            selectedTab : 'store-resources',
            storedResources : [
                {
                    id:'official-guide' ,
                    tittle : 'Official Guide' ,
                    description : 'The official documentation' ,
                    link : 'https://vuejs.org'
                },
                {
                    id:'google' ,
                    tittle : 'Official google' ,
                    description : 'Learn to google....' ,
                    link : 'https://www.google.com'
                },
            ]
        }
    },
    provide () {
        return {
            resources : this.storedResources,
            addResource : this.addResource,
            deleteResource : this.removeResource
        };
    },
    computed : {
        storedResButtonMode() {
            return this.selectedTab === 'store-resources' ? null : 'flat';
        },
        addResButtonMode() {
            return this.selectedTab === 'add-resource' ? null : 'flat';
        }
    },
    methods: {
        setSelectedTab(tab) {
            this.selectedTab = tab;
        },
        addResource (title , description , link) {
           const newResource = {
              id: Date.now(),
              tittle: title,
              description : description,
              link : link
           }
           this.storedResources.unshift(newResource)
           this.selectedTab = 'store-resources'
        },
        removeResource(resId) {
            const resIndex = this.storedResources.findIndex(res => res.id === resId)
            this.storedResources.splice(resIndex , 1)
        }
    },
})
</script>
