<script>
    import { onMount } from 'svelte';
    import 'bootstrap/dist/css/bootstrap.min.css';

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
            text: 'Question 6: I have difficulties with sleep (e.g., waking early, finding it hard to go to sleep)', 
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
    <h1 style="font-family: Futura; font-size: 34px; margin-bottom: 20px; line-height: 2;">Anxiety Self-Assessment Quiz</h1>
    {#if currentQuestionIndex <= 9}
        
        <div style="font-family: Verdana;">
            <h2 style="font-size: x-large; font-weight: 100; font-size: 30px; line-height: 3; margin-left: 1%;">{questions[currentQuestionIndex].text}</h2>
            {#each questions[currentQuestionIndex].options as option, index}
                <button type="button" class="btn btn-light" on:click={() => selectOption(index)}>{option}</button>
            {/each}
        </div>
        
        <div>
            <!-- <progress value={currentQuestionIndex + 1} max={questions.length}></progress> -->
            <div class="progress" style="width: 50%; margin: 0 auto; margin-top: 2%;">
                <div class="progress-bar" role="progressbar" style="width: {(currentQuestionIndex + 1) * 10}%; background-color: #e3c466;"></div>
              </div>
            <span>Question {currentQuestionIndex + 1} of {questions.length}</span>
        </div>
        
    {:else}
        <div style="font-family: Verdana, sans-serif; line-height: 10; margin-bottom: 50px;">
            <h2>Your total score is: {totalPoints}</h2>
            <!-- Button to retake the quiz -->
        </div>

        <!-- Display the three categories and the progress bar -->
        <div style="display: flex; flex-direction: column; margin-top: 20px; font-family: Verdana, sans-serif; width: 70%; margin: 0 auto;">
            
            <div style="position: relative; ">
                <div style="position: absolute; 
                            top: 0; 
                            left: calc({totalPoints / 40 * 99}% - 5px); 
                            width: 25px; 
                            height: 25px; 
                            background-color: #fce6a4; 
                            border-radius: 50%; 
                            border: 3px solid #7d7559; 
                            transform: translateY(-40%);">
                </div>
                <div style="width: 99%; justify-content: center; height: 6px; background-color: #d9d7d2;"></div>
            </div>
            <div style="display: flex; justify-content: space-between; margin-bottom: 10px; line-height: 5">
                <p style="flex: 1; text-align: left;">Unlikely (0-10)</p>
                <p style="flex: 1; text-align: center;">May be suffering (11-26)</p>
                <p style="flex: 1; text-align: right;">Likely (27-40)</p>
            </div>
        </div>

        <button type="button" class="btn btn-light" style="margin-top: 5%;" on:click={resetQuiz}>Retake the Quiz</button>
    {/if}  
</main>



<style>
    button {
        margin: 5px;
    }
</style>

