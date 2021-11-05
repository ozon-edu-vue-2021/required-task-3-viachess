<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
      <HomeIconSVG class="person__photo-home" />
    </div>
    <div class="person__info">
      <h4 class="person__info-name">{{ person.name }}</h4>
      <h4 class="person__info-age">Возраст: {{ person.age }}</h4>
      <h4 class="person__info-registered">
        Дата регистрации: {{ formatedDate }}
      </h4>
      <a class="person__info-email" :href="['mailto:' + person.email]">
        <EmailSVG class="person__info-email__icon" />
      </a>
    </div>
    <div class="person__info-about">
      <h3 class="person__info-about__heading">О себе</h3>
      <p>
        {{ person.about }}
      </p>
    </div>
  </div>
</template>

<script>
import HomeIconSVG from "@/assets/images/home_icon.svg";
import EmailSVG from "@/assets/images/email_icon.svg";
import parseISO from "date-fns/parseISO";

export default {
  props: {
    person: {
      type: Object,
      default: null,
    },
  },
  components: {
    HomeIconSVG,
    EmailSVG,
  },
  computed: {
    formatedDate() {
      return parseISO(this.person.registered);
    },
  },
};
</script>

<style scoped>
.person {
  display: grid;
  grid-template-columns: 120px 1fr;
  grid-template-rows: min-content min-content;
  grid-gap: 8px;
  overflow: hidden;
}

.person__photo {
  position: relative;
}

.person__photo img {
  width: 120px;
  border-radius: 10px;
}

.person__photo-home {
  fill: #f4f2f3;
  cursor: pointer;
  position: absolute;
  width: 32px;
  height: 32px;
  border-radius: 10px;
  box-shadow: 2px 3px 6px 3px rgba(34, 60, 80, 0.2);
  left: 10px;
  bottom: 10px;
  background-color: rgba(107, 113, 126, 0.6);
  opacity: 0.8;
  transition: all 0.2s ease;
}
.person__photo-home:hover {
  opacity: 1;
  box-shadow: 2px 3px 6px 3px rgba(34, 60, 80, 0.4);
  transform: translateY(-2px);
}

.person__info {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.person__info-email {
  display: flex;
  justify-content: center;
  border-radius: 10px;
  width: 40px;
  height: 30px;
  background-color: rgba(33, 107, 255, 0.1);
  transition: all 0.2s ease;
}
.person__info-email:hover {
  transform: translateY(-2px);
}

.person__info-email__icon {
  width: 30px;
  height: 30px;
  fill: #216bff;
}

.person__info-name {
  color: #216bff;
}

.person__info-registered {
  color: grey;
  font-weight: normal;
}

.person__info-about {
  grid-column: 1 / -1;
  padding-bottom: 10px;
}

.person__info-about__heading {
  padding-top: 10px;
  padding-bottom: 10px;
}

.person__info-about p {
  line-height: 1.3rem;
}
</style>
