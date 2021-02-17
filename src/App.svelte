<script>
  import Folder from './Folder.svelte';
  import { render_list } from './stores.js';
  // import tree from './tree.json'
  // console.log(tree)
  // tree.name='.'
  let tree, url;
  let github_repo, branch_name, prefix;

  console.log('query string', window.location.search)
  var urlParams = new URLSearchParams(window.location.search);
  if (urlParams.has('url')) {
    url = urlParams.get('url')
    console.log('got url from params', url);
    prefix = url.split('web/output.json')[0]
  } else {
    url = 'https://raw.githubusercontent.com/saewoonam/random-data/main/file_tree.json'
    github_repo = 'saewoonam/sc-current-source-hw'
    branch_name = 'main'
    prefix = "https://raw.githubusercontent.com/"+github_repo;
    prefix += "/"+branch_name+"/"
  }
  // console.log('encode', encodeURIComponent(url))
  // https%3A%2F%2Fraw.githubusercontent.com%2Fsaewoonam%2Frandom-data%2Fmain%2Ffile_tree.json
  async function load_data(url) {
    let response = await fetch(url)
    let text = await response.text()
    tree = JSON.parse(text);
    tree.name = 'got kicad files to show'
    console.log('got tree', tree)
    }
  load_data(url)

  $: {
      if ($render_list.length > 0) {
        console.log($render_list)
        // build src for iframe
        let kicad_list = encodeURIComponent($render_list.map(a => prefix+a.path).join('\n'))
        let src_url = "https://saewoonam.github.io/kicad-utils/viewer.html?url="+kicad_list;
        console.log('src_url', src_url);
        const link = document.createElement('a');
        link.href = src_url
        document.body.appendChild(link)
        link.dispatchEvent(new MouseEvent('click'))
      }
  }
</script>
{#if tree}
<Folder {...tree}/>
<p>
  render_list: {JSON.stringify($render_list)}
</p>
{/if}
