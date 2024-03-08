<script>
    import { onMount } from 'svelte';

    let questions = [
        { 
            text: 'Question 1: I find it very hard to unwind, relax or sit still', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 2: I have had stomach problems, such as feeling sick or stomach cramps', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 3: I have been irritable and easily become annoyed', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 4: I have experienced shortness of breath', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 5: I have felt dizzy and unsteady at times', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 6: I have had difficulties with sleep (e.g., waking early, finding it hard to go to sleep)', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 7: I have felt panicked and overwhelmed by things in my life', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 8: I have felt nervous and on edge', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 9: I have had trembling hands', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
        { 
            text: 'Question 10: I seem to be constantly worrying about things', 
            options: ['Never', 'Rarely', 'Sometimes', 'Often', 'Very often']
        },
    ];

    let totalPoints = 0;
    let currentQuestionIndex = 0;

    const selectOption = (points) => {
        totalPoints += points;
        if (currentQuestionIndex <= 9) {
            currentQuestionIndex++;
        }
    };

    // Function to reset the quiz
    const resetQuiz = () => {
        totalPoints = 0;
        currentQuestionIndex = 0;
    };
</script>

<main>
    <h1 style="font-family: Verdana">Self-test quiz</h1>
    {#if currentQuestionIndex <= 9}
        <div>
            <progress value={currentQuestionIndex + 1} max={questions.length}></progress>
            <span>Question {currentQuestionIndex + 1} of {questions.length}</span>
        </div>

        <div style="font-family: Verdana, sans-serif;">
            <h2 style="font-size: extra larger;">{questions[currentQuestionIndex].text}</h2>
            {#each questions[currentQuestionIndex].options as option, index}
                <button on:click={() => selectOption(index)}>{option}</button>
            {/each}
        </div>
        
    {:else}
        <div style="font-family: Verdana, sans-serif;">
            <h2>Your total score is: {totalPoints}</h2>
            <!-- Button to retake the quiz -->
            <button on:click={resetQuiz}>Retake the Quiz</button>
        </div>
    {/if}

    <!-- Display the three categories and the progress bar -->
    <div style="display: flex; flex-direction: column; margin-top: 20px; font-family: Verdana, sans-serif;">
        <div style="display: flex; justify-content: space-between; margin-bottom: 10px;">
            <p>Unlikely (0-10)</p>
            <p>Might be suffering (11-26)</p>
            <p>Likely (27-40)</p>
        </div>
        <div style="position: relative;">
            <div style="position: absolute; top: 0; left: calc({totalPoints / 40 * 100}% - 5px); width: 25px; height: 25px; background-color: #fce6a4; border-radius: 50%; border: 3px solid #7d7559; transform: translateY(-40%);"></div>
            <div style="width: 100%; height: 6px; background-color: #d9d7d2;"></div>
        </div>
    </div>
</main>


<style>
    button {
        margin: 5px;
    }
</style>

