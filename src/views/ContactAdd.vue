<template>
    <div
        v-if="contact"
        class="page"
    >
        <h4>Thêm Liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="onCreateContact"
        />
        <p>{{ message }}</p>
    </div>
</template>

<script>
import ContactForm from '@/components/ContactForm.vue';
import { contactService } from '@/services/contact.service';

export default {
    components: {
        ContactForm,
    },
    props: {
        contactId: { type: Number, required: true },
    },
    data() {
        return {
            contact: {
                name: '',
                email: '',
                address: '',
                phone: '',
                favorite: 0
            },
            message: '',
        };
    },
    methods: {
        async getContact(id) {
            try {
                this.contact = await contactService.get(id);
            } catch (error) {
                console.log(error);
                this.$router.push({
                    name: 'notfound',
                    params: {
                        pathMatch: this.$router.path.split('/').slice(1)
                    },
                    query: this.$route.query,
                    hash: this.$route.hash,
                });
            }
        },

        async onCreateContact(contact) {
            try {
                await contactService.create(contact);
                this.message = 'Liên hệ được thêm thành công';
            } catch(error) {
                console.log(error);
            }
        },
    },

    created() {
        this.contact = {
            name : '',
            email : '',
            address : '',
            phone : '',
            favorite : null
        };
        this.message = '';
    },
};
</script>