<script setup lang="ts">
import Button from 'primevue/button';
import Splitter from 'primevue/splitter';
import SplitterPanel from 'primevue/splitterpanel';
import Tree from 'primevue/tree';
import Breadcrumb from 'primevue/breadcrumb';
import Divider from 'primevue/divider';
import { ref } from 'vue';
import Panel from 'primevue/panel';
import MarkdownIt from 'markdown-it';

const md = new MarkdownIt()
const text = ref<string>(`
# 自述文件

自述文件文本内容

## 二级标题

\`\`\`bash
代码块
\`\`\`

- 列
- 列

`)
const nodes = ref([
  {
    key: '0',
    label: 'Documents',
    data: 'Documents Folder',
    icon: 'pi pi-folder',
    children: [
      {
        key: '0-0',
        label: 'Work',
        data: 'Work Folder',
        icon: 'pi pi-fw pi-folder',
        children: [
          { key: '0-0-0', label: 'Expenses.doc', icon: 'pi pi-fw pi-file', data: 'Expenses Document' },
          { key: '0-0-1', label: 'Resume.doc', icon: 'pi pi-fw pi-file', data: 'Resume Document' }
        ]
      },
      {
        key: '0-1',
        label: 'Home',
        data: 'Home Folder',
        icon: 'pi pi-fw pi-home',
        children: [{ key: '0-1-0', label: 'README.md', icon: 'pi pi-fw pi-file', data: 'Invoices for this month' }]
      }
    ]
  }
],)

const showFiles = ref<boolean>(true)

const home = ref({
  icon: 'pi pi-home'
});
const items = ref([
  { label: 'Documents' },
  { label: 'Home' },
  { label: 'README.md' },
]);

const size = ref(25)
</script>

<template>
  <main>
    <Splitter class="w-full h-full">
      <SplitterPanel v-if="showFiles" class="flex align-items-center justify-content-center" :size="size">
        <div class="h-full" style="width: calc(100%);">
          <div class="flex flex-column gap-3 pt-3">
            <div class="flex flex-row gap-3 pl-2 align-items-center">
              <Button @click="showFiles = !showFiles" icon="pi pi-angle-double-left" outlined size="small"></Button>
              <span class="font-bold">Files</span>
            </div>
            <Tree :value="nodes" :filter="true" filterMode="lenient"></Tree>
          </div>
        </div>
      </SplitterPanel>
      <SplitterPanel class="flex align-items-center justify-content-center" :size="75">
        <div class="w-full h-full p-3">
          <div class="flex flex-column align-items-start justify-content-center">
            <div class="flex flex-row gap-1">
              <div v-if="!showFiles" class="flex align-items-center justify-content-center">
                <Button @click="showFiles = !showFiles" icon="pi pi-angle-double-right" outlined size="small"></Button>
              </div>
              <Breadcrumb :home="home" :model="items" />
            </div>
            <Divider></Divider>
            <div class="w-full">
              <Panel header="自述文件">
                <div class="md">
                  <div v-html="md.render(text)"></div>
                </div>
              </Panel>
            </div>
          </div>
        </div>
      </SplitterPanel>
    </Splitter>
  </main>
</template>

<style scoped>
:deep(.p-splitter) {
  border-radius: 0;
}

:deep(.p-treenode-label) {
  overflow-x: hidden;
  text-overflow: ellipsis;
}

:deep(.p-tree .p-tree-container .p-treenode .p-treenode-content) {
  margin: 0;
  padding-left: 0;
  padding-right: 0;
  padding-bottom: 0;
}

:deep(.p-tree .p-treenode-children) {
  padding: 0 0 0 0.5rem;
}

main {
  height: 100%;
  width: 100%;
}
</style>

<style>
.md code {
  background-color: aliceblue;
  padding: 3px;
  border-radius: 1rem;
}
</style>
