<template>
  <div id="app">
    <!-- 스토리 모드일 때 스토리와 선택지를 표시 -->
    <StoryComponent v-if="isStoryMode" :story="currentStory" />
    <ChoiceComponent v-if="isStoryMode" :choices="currentChoices" @choose="handleChoice" />
    
    <!-- 전투 모드일 때 일기토 화면 표시 -->
    <BattleComponent v-else @endBattle="handleBattleEnd" />
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import StoryComponent from './components/StoryComponent.vue';
import ChoiceComponent from './components/ChoiceComponent.vue';
import BattleComponent from './components/BattleComponent.vue';

export default {
  name: 'App',
  components: {
    StoryComponent,
    ChoiceComponent,
    BattleComponent,
  },
  setup() {
    const storyTable = ref([
      { id: 1, text: "예수게이의 죽음 이후... 벡테르와 테무진의 다툼이 심화되었습니다.", next_story_id: 2 },
      { id: 2, text: "그후로 테무진은..", next_story_id: null },
    ]);

    const choicesTable = ref([
      { id: 1, story_id: 1, choice_text: "벡테르와 한판 붙어보자", next_story_id: 2 },
    ]);

    const playerProgress = ref({ player_id: 'player_1', current_story_id: 1 });
    const isBattleMode = ref(false);

    const getCurrentStory = () => {
      return storyTable.value.find(story => story.id === playerProgress.value.current_story_id);
    };

    const getChoicesForCurrentStory = () => {
      return choicesTable.value.filter(choice => choice.story_id === playerProgress.value.current_story_id);
    };

    const currentStory = ref(getCurrentStory());
    const currentChoices = ref(getChoicesForCurrentStory());

    const handleChoice = (choiceId) => {
      const selectedChoice = choicesTable.value.find(choice => choice.id === choiceId);
      if (selectedChoice) {
        if (selectedChoice.choice_text === "벡테르와 한판 붙어보자") {
          isBattleMode.value = true;  // 일기토 모드로 전환
        } else {
          playerProgress.value.current_story_id = selectedChoice.next_story_id;
          currentStory.value = getCurrentStory();
          currentChoices.value = getChoicesForCurrentStory();
        }
      }
    };

    const handleBattleEnd = () => {
      isBattleMode.value = false;
      playerProgress.value.current_story_id = 2;  // 전투 종료 후 다음 스토리로 진행
      currentStory.value = getCurrentStory();
      currentChoices.value = getChoicesForCurrentStory();
    };

    return {
      currentStory,
      currentChoices,
      handleChoice,
      handleBattleEnd,
      isStoryMode: computed(() => !isBattleMode.value),  // 스토리 모드와 전투 모드를 구분
    };
  },
};
</script>
