<template>
    <span :class="{'nova-button-loading': loading}">
        <button 
            @click="handleClick"
            :class="field.classes"
            class="nova-button"
        >{{ field.text }}</button>
    </span>
</template>

<style>
    .nova-button-loading {
        pointer-events: none;
        opacity: 0.5;
    }
</style>

<script>
export default {
    props: ['resource', 'resourceName', 'resourceId', 'field'],
    methods: {
        async handleClick() {
            
            try {
                
                const response = await this.post();

                this.loading = false;

                this.$emit('clicked');

                this.$toasted.show(
                    this.__(this.field.successMessage),
                    {type: 'success'}
                );
            } catch (error) {
                this.$toasted.show(
                    this.__(this.field.errorMessage),
                    {type: 'error'}
                );
            }
        },
        post()
        {
            this.loading = true;

            let root = '/nova-vendor/nova-button/';

            return Nova.request().post(root + `${this.resourceName}/${this.resourceId}/${this.field.key}/`, {event: this.field.event});
        }
    }
}
</script>