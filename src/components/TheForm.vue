<template>
  <div>
    <p>Город:</p>
    <select v-model="model.city">
      <option disabled :value="null">Выберите город</option>
      <option :selected="isSelectedCity" v-for="(city, idx) in cities" :value="city.id" :key="idx">
        {{ city.name }}
      </option>
    </select>
  </div>
  <div>
    <p>Цех:</p>
    <select v-model="model.department" :disabled="isDepSelDisabled">
      <option disabled :value="null">Выберите цех</option>
      <option v-for="(department, idx) in deps" :value="department.id" :key="idx">{{ department.name }}</option>
    </select>
  </div>
  <div>
    <p>Сотрудник:</p>
    <select v-model="model.employee" :disabled="isEmpSelDisabled">
      <option disabled :value="null">Выберите сотрудника</option>
      <option v-for="(employee, idx) in emp" :value="employee.id" :key="idx">{{ employee.name }}</option>
    </select>
  </div>
  <div>
    <p>Бригада:</p>
    <select v-model="model.brigade" :disabled="isEmpSelDisabled">
      <option disabled :value="null">Выберите бригаду</option>
      <option value="0">I</option>
      <option value="1">II</option>
      <option value="3">III</option>
    </select>
  </div>
  <div>
    <p>Смена:</p>
    <select v-model="model.shift" :disabled="isEmpSelDisabled">
      <option disabled :value="null">Выберите смену</option>
      <option value="0">1</option>
      <option value="1">2</option>
    </select>
  </div>
  <button ref="submitButton" @click="setCookie">Выбрать</button>
</template>

<script setup>
  import cities from "@/data/cities.json";
  import departments from "@/data/departments.json";
  import employees from "@/data/employees.json";

  import { ref, watch } from "vue";

  const deps = ref();
  const emp = ref();

  const isDepSelDisabled = ref(true);
  const isEmpSelDisabled = ref(true);

  const model = ref({
    city: null,
    department: null,
    employee: null,
    brigade: null,
    shift: null,
  });

  watch(
    () => model.value.city,
    (newValue) => {
      const _deps = departments.filter(({ cityId }) => newValue === cityId);
      deps.value = _deps;
      isDepSelDisabled.value = false;
      model.value.department = null;
    }
  );

  watch(
    () => model.value.department,
    (newValue) => {
      const _emp = employees.filter(({ departmentId }) => newValue === departmentId);
      emp.value = _emp;
      isEmpSelDisabled.value = false;
      model.value.employee = null;
    }
  );
  const setCookie = () => {
    document.cookie = `formData=${JSON.stringify(model.value)}`;
  };
</script>
