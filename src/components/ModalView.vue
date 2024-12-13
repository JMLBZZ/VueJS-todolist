<template>
    <div 
        class="modal fade" 
        id="addModal" 
        tabindex="-1" 
        aria-hidden="true"
        data-bs-backdrop="false"
    >
        <div class="modal-dialog modal-sm modal-dialog-top-right">
            <div class="modal-content bg-success">
                <div class="modal-header">
                    <h5 class="modal-title" id="successModalLabel">
                        <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="50" height="50" viewBox="0,0,300,300">
                            <g fill="#ffffff" fill-rule="nonzero" stroke="none" stroke-width="1" stroke-linecap="butt" stroke-linejoin="miter" stroke-miterlimit="10" stroke-dasharray="" stroke-dashoffset="0" font-family="none" font-weight="none" font-size="none" text-anchor="none" style="mix-blend-mode: normal">
                                <g transform="scale(5.12,5.12)">
                                    <path d="M43.171,10.925l-19.086,22.521l-9.667,-9.015l1.363,-1.463l8.134,7.585l17.946,-21.175c-4.204,-4.534 -10.205,-7.378 -16.861,-7.378c-12.683,0 -23,10.317 -23,23c0,12.683 10.317,23 23,23c12.683,0 23,-10.317 23,-23c0,-5.299 -1.806,-10.182 -4.829,-14.075z"></path>
                                </g>
                            </g>
                        </svg>
                    </h5>
                    <button 
                        type="button" 
                        class="btn-close btn-close-white" 
                        data-bs-dismiss="modal" 
                        aria-label="Fermer"
                    ></button>
                </div>
                <div class="modal-body text-white">
                    La tâche <strong>"{{ taskTitle }}"</strong> a été ajoutée avec succès !
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ModalView",
    props: {
        taskTitle: {
            type: String,
            required: true,
        },
    },
    mounted() {
        console.log("MODALVIEW EST MONTÉ");
    },
};
</script>

<style scoped>
    .modal-dialog-top-right {
        position: absolute;
        top: 10px;
        right: 10px;
        margin: 0;
        pointer-events: auto;
    }

    .bg-success {
        background-color: #00dd42 !important;/** Vert avec transparence */
    }
</style>
