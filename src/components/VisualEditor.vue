<template>
    <div class="visualeditor-container">
    </div>
</template>

<script>
/* global $, ve */
export default {
    name: 'VisualEditor',
    props: {
        initialContent: String,
    },
    methods: {
        async getContent() {
            // This is async to represent how in a MW-VE component, this would
            // probably be doing an API call to extract the wikitext value.
            return this.target.getSurface().getHtml();
        },
        setContent( html ) {
            // Create a document model for a new surface
            this.target.clearSurfaces();
            this.target.addSurface(
                ve.dm.converter.getModelFromDom(
                    ve.createDocumentFromHtml( html ),
                    // Optional: Document language, directionality (ltr/rtl)
                    { lang: $.i18n().locale, dir: $( document.body ).css( 'direction' ) }
                )
            );
        }
    },
    mounted() {
        var $instance = $( this.$el );

        new ve.init.sa.Platform( ve.messagePaths ).getInitializedPromise()
            .fail( function () {
                $instance.text( 'Sorry, this browser is not supported.' );
            } )
            .done( () => {
                this.target = new ve.init.sa.Target();

                // Append the target to the document
                $instance.append( this.target.$element );

                this.setContent( this.initialContent || '<p><b>Hello,</b> <i>World!</i></p>' );
            } );
    },
    beforeDestroy() {
        if (this.target) {
            this.target.destroy();
            this.target = null;
        }
    },
};
</script>

<style>
.visualeditor-container {
    text-align: left;
}
</style>