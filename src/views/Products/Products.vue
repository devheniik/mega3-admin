<template>
<div>
    <Navigator @new="$router.push({ name: 'productnew' })" title="Product" :actions="[{ title: 'New', emit: 'new', color: 'indigo-500' }]" />
    <div class="lg:mx-6 lg:my-3 grid grid-cols-3 gap-4" >
        Table
    </div>
</div>
    
</template>

<script>
export default {
    setup(props) {
    }
}
</script>