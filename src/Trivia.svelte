<script>
    import { Button, Card, CardBody, CardFooter, CardHeader, CardSubtitle, CardText, CardTitle } from 'sveltestrap';
    import { Form, FormGroup } from 'sveltestrap';

    export let category;
    export let difficulty;
    
    if (category === 'Video Games') {
        category = 15;
    } else if (category === 'Mythology') {
        category = 20;
    } else if (category === 'Geography') {
        category = 22;
    } else {
        console.log('category does not exist here', category)
    }
    
    let triviaPromise = computeAPI(category, difficulty);

    async function computeAPI(cat, diff) {
        let APIurl = `https://opentdb.com/api.php?amount=10&category=${cat}&difficulty=${diff}&type=multiple`;
        let response = await fetch(APIurl);
        let data = await response.json();
        console.log(data)
        return data;
    }

    let configure_question_data = (question) => {
        question.incorrect_answers.push(question.correct_answer)
        question.incorrect_answers.sort()
    }

    let current_question = 0;
    const next_question = () => {
        current_question += 1;
        check_disabled = false;
        true_banner = false;
        false_banner = false;
        disabled = true;
    }

    let done = false;
    $: if (current_question > 9) {
        console.log('MORE THAN 9!!!!!!!!')
        done = true;
    }

    let disabled = true;
    let check_disabled = false;
    let true_banner = false;
    let false_banner = false;
    let correct_questions = 0;
    let incorrect_questions = 0;
    let selectedTrivia;
    let correct_answer_banner;
    // $: console.log('Change selected:', selected)
    const check_answer = (correct_answer) => {
        correct_answer_banner = correct_answer;
        disabled = false;
        check_disabled = true;
        console.log(selectedTrivia, correct_answer)
        if (selectedTrivia === correct_answer) {
            true_banner = true;
            correct_questions += 1;
        } else {
            false_banner = true;
            incorrect_questions += 1;
        }
    }
</script>

{#if !done}
<div class="trivia-board">
    <div class="numbered-questions">
        <h4>Question: {current_question + 1}/10</h4>
        <div class="correct-incorrect">
            <h5 class="first-h5" style="color: green">Correct: {correct_questions}</h5>
            <h5 style="color: crimson">Incorrect: {incorrect_questions}</h5>
        </div>
    </div>
    {#await triviaPromise}
        Loading your trivia data...
    {:then trivia}
        {#each trivia.results as question}
            {#if trivia.results.indexOf(question) === current_question}
                <Card>
                    {#if true_banner}
                        <div class="banner true">Correct!</div>
                    {/if}
                    {#if false_banner}
                        <div class="banner false">Incorrect! The correct answer was {correct_answer_banner}</div>
                    {/if}
                    <CardHeader>
                        <CardTitle>{question.question}</CardTitle>
                    </CardHeader>
                    <CardBody>
                        <CardSubtitle><i>Multiple Choice</i></CardSubtitle>
                        <CardText>
                            <Form>
                                <FormGroup>
                                    <p hidden>{configure_question_data(question)}</p>
                                    <div class="radio-group">
                                        {#each question.incorrect_answers as value}
                                        <div>
                                            <input type="radio" value={value} bind:group={selectedTrivia}>
                                            {value}
                                        </div>
                                    {/each}
                                    </div>
                                </FormGroup>
                            </Form>
                        </CardText>
                    </CardBody>
                    <CardFooter>
                        <Button disabled={check_disabled} on:click={() => check_answer(question.correct_answer)} type="button">Check</Button>
                        <Button color="primary" type="button" on:click={next_question} disabled={disabled}>Next</Button>
                    </CardFooter>
                </Card>
            {/if}
        {/each}
    {/await}
</div>
{:else if done}
<div class="all_done">
    <h1>{correct_questions >= 5 ? 'Congratulations' : 'Bummer'}!</h1>
    <p>You got <span style="color: green"><b>{correct_questions}</b></span> correct and <span style="color: crimson"><b>{incorrect_questions}</b></span> wrong!</p>
    <p>Click on the Trivia Game link in the top left to try again!</p>
</div>
{/if}

<style>
    .trivia-board {
        width: 500px;
        margin: 0 auto;
    }

    .banner {
        height: 50px;
        width: 100%;
        padding-left: 10px;
        color: black;
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }
    .true {
        background-color: lightgreen;
    }
    .false {
        background-color: lightcoral;
    }

    .radio-group {
        display: flex;
        flex-direction: column;
    }

    .numbered-questions {
        display: flex;
        justify-content: space-between;
        margin: 5px 10px;
    }
    .numbered-questions h4 {
        width: 300px;
    }
    .correct-incorrect {
        display: flex;
        width: 600px;
        justify-content: flex-end;
    }
    .first-h5 {
        color: green;
        padding-right: 15px;
    }

    .all_done {
        width: 400px;
        text-align: center;
        margin: 0 auto;
    }
</style>