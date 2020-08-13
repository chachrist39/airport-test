<template>

    <!-- เมนูบน PC หรือ อุปกรณ์สื่อสารที่มีหน้าจอขนาดใหญ่ -->
    <v-app-bar dark>
        <v-toolbar-title>
            <font size="6">KI<font color="orange">O</font>SK BANKING</font><font size="2"> {{version}}</font>
        </v-toolbar-title>
    </v-app-bar>
</template>
<script>
export default {
    data(){
        return{
            version: 'v.1.0.0'
        }
    },
}
</script>