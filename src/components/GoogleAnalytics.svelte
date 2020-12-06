	<script>
        import { stores } from '@sapper/app';
        const { page } = stores();
        $: {
            if (typeof gtag !== 'undefined'){
                gtag('js', new Date());
                gtag("config", "G-T5JLQJ81VH", {
                    page_path: $page.path
                });
            }
        }
	</script>