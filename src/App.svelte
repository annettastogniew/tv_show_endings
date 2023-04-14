<script>
  import IconButton, { Icon } from '@smui/icon-button';
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';
  import Grid from './lib/Grid.svelte';
  import Question from './lib/Question.svelte';

  let showImage = false;
  let questionCount = 1;
  let shows = [];
  let selectedShow = {};
  let selectedEnding = '';
  let selectedRevival = '';
  let question1Text = '';
  let question2Text = '';
  let question1Options = [{'at just the right time': 'well-timed'}, {'too soon': 'out of nowhere'}, {'past its prime': 'long overdue'}];
  let question2Options = [{'yes': 'for sure'}, {'no': 'no way'}];
  let endingResponsiveText = '';
  let revivalResponsiveText = '';

  const showStatic = () => {
    showImage = true;
    setTimeout(() => {
      document.getElementById('questions-content').classList.remove('hidden');
      document.getElementById('question-1').scrollIntoView();
    }, 950);
    setTimeout(() => {
      showImage = false;
    }, 750);
  };

  const setResponsiveText = event => {
    if (questionCount === 3) {
      selectedEnding = event.detail.text;
      console.log(event.detail.text);
      console.log(selectedEnding);
      console.log(selectedShow.cat);
      endingResponsiveText = selectedEnding === selectedShow.cat ? 'Most people agree.' : 'Most people actually think it ended ' + selectedShow.cat + '.';
      nextQuestion();
    } else {
      selectedRevival = event.detail.text;
      revivalResponsiveText = selectedRevival === 'yes' ? "That's not suprising." : "Interesting. Some super fans might not agree.";
      toResults();
    }
  }

  const nextQuestion = () => {
    questionCount += 1;
    const nextQuestion = document.getElementById('question-' + questionCount);
    nextQuestion.classList.remove('hidden');
    nextQuestion.scrollIntoView();
  };

  const showDetails = show => {
    nextQuestion();
    selectedShow = show;
    question1Text = "First, tell us a little bit about " + selectedShow.name + ". What did you think of " + selectedShow.name + "'s ending?";
    question2Text = "And be honest, if you could bring back " + selectedShow.name + ", would you?";
  };

  const toResults = () => {
    const resultOne = document.getElementById('results-1');
    resultOne.classList.remove('hidden');
    resultOne.scrollIntoView();
  }

  onMount(async () => {
		const res = await fetch('src/data/show_ratings.json');
		shows = await res.json();
	});
</script>

<main>
  <section>
    <div>
      <h1>
        When To Call Cut
      </h1>
      <h2>
        The stories of TV shows that were cut short, aired for one too many seasons and everything in between
      </h2>
      <p class="byline">By Annetta Stogniew</p>
      <IconButton on:click={showStatic} color="secondary">
        <Icon class="material-icons">play_arrow</Icon>
      </IconButton>
    </div>
  </section>
  {#if showImage}
    <img transition:fade src="src/assets/tv_static.gif" alt="tv static" class="fullscreen" id="tv-static">
  {/if}
  <div id="questions-content" class="hidden">
    <section id="question-1">
      <p>
        In the post-cable age, many seemingly ancient shows have seen resurgences in popularity. Some shows anchored
        such a fervent fanbase that fans actually managed to 
        <a href="https://www.insider.com/fans-saved-tv-show-2018-6">bring the show back</a>. Other shows have not been so fortunate.
      </p>
      <p>
        We've gathered some shows that were brutally cancelled, others that probably should've 
        ended in their prime and a few with perfectly-executed story archs.
      </p>
      <p>
        Click next to see how your favorite shows rank.
      </p>
      <p>
        <strong>
          Click next to see a list of shows. Select your favorite show from the list to learn more about its ending.
        </strong>
      </p>
      <div>
        <button on:click={nextQuestion}>
          Next
        </button>
      </div>
    </section>
    <section id="question-2" class="hidden">
      {#if shows.length > 0}
        <Grid shows={shows} showDetails={showDetails}/>
      {/if}
    </section>
    <section id="question-3" class="hidden">
      <Question questionText={question1Text} options={question1Options} group={selectedEnding} on:logChoice={setResponsiveText} />
    </section>
    <section id="question-4" class="hidden">
      <Question questionText={question2Text} options={question2Options} group={selectedRevival} on:logChoice={setResponsiveText} />
    </section>
  </div>
  <div id="results-content">
    <section id="results-1" class="hidden">
      {#if endingResponsiveText && revivalResponsiveText}
        <p>Ok, you rated {selectedShow.name} as having ended {selectedEnding}. {endingResponsiveText}</p>
      {/if}
    </section>
  </div>
</main>

<style>
  section {
    height: 100vh;
    max-height: 100vh;
    max-width: 50vw;
    display: flex;
    justify-content: center;
    flex-direction: column;
  }

  .fullscreen {
    height: 100vh;
    width: 100vw;
    right: 0;
    top: 0;
    position: absolute;
  }

  .hidden {
    display: none;
  }

  .byline {
    text-align: center;
  }
</style>
