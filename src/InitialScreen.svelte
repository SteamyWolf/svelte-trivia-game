<script>
    import { Button, Card, CardBody, CardFooter, CardHeader, CardSubtitle, CardText, CardTitle } from 'sveltestrap'; 
    import { Form, FormGroup, FormText, Input, Label } from 'sveltestrap';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();


  let params = {
      category: '',
      difficulty: ''
  }

  export const onSubmit = () => {
    params.difficulty = params.difficulty.toLowerCase();
    dispatch('continue', params);
  }

</script>

<div class="choose-difficulty">
	<Card>
		<CardHeader>
			<CardTitle>Please choose a <b>Category</b> and <b>Difficulty</b></CardTitle>
		</CardHeader>
		<CardBody>
            <CardSubtitle style="margin-bottom: 10px"><i><b>* Please fill out both fields</b></i></CardSubtitle>
            <CardText>
                <Form>
                    <FormGroup>
                        <Label for="category">Category</Label>
                            <Input type="select" name="select" id="category" bind:value={params.category}>
                                <option selected="selected">* Select a category *</option>
                                <option>Video Games</option>
                                <option>Mythology</option>
                                <option>Geography</option>
                            </Input>
                    </FormGroup>
                    <FormGroup>
                        <Label for="difficulty">Difficulty</Label>
                            <Input type="select" name="select" id="difficulty" bind:value={params.difficulty}>
                                <option selected="selected">* Select a difficulty *</option>
                                <option>Easy</option>
                                <option>Medium</option>
                                <option>Hard</option>
                            </Input>
                    </FormGroup>
                </Form>
            </CardText>
        </CardBody>
        <CardFooter class="card-footerr">
            <Button on:click={onSubmit} type="submit" color="primary" disabled={!params.category || !params.difficulty || params.difficulty === '* Select a difficulty *' || params.category === '* Select a category *'}>Continue</Button>
        </CardFooter>
	</Card>
	</div>



    <style>
        .choose-difficulty {
            display: flex;
            justify-content: center;
        }
    </style>