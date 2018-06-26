### Thematization of propositions of the Brazilian Deputies Chamber with Machine Learning algorithms

**Authors**: Fernanda Scovino (w/ collaborators: Alifer Sales, Beatriz Coimbra e João Carabetta)

---

The main problem is to generate themes of proposition based on their "abstract", found in the field `ementa`. Our database is composed by the propositions of legal significant, that means propositions that can change legislation*, from 1988 until now: 53% of those propositions don't have themes. This is a **multilabel problem**: one proposition can have more than one theme.

The data is originally from the [Open Data Portal of Deputies Chamber](http://www2.camara.leg.br/transparencia/dados-abertos/dados-abertos-legislativo/webservices/proposicoes-1/obterproposicaoporid), and was captured and treated by me and others *Congresso em Números* researchers.

\*the types are PEC, PLP, PL, MPV, PLV, PDC.


**Methodology**

There are 42 differents themes that we've grouped into 4 macrothemes.

Once we have created the macrothemes, our approach to deal with the multilabel problem was a *Label Powerset* transformation: each combination of themes become a new class, so we wen't from 4 to 16 ($2^{4}$) categories, changing our target function.

After preprocessing the data, we've separeted train (80%) and test (20%) datasets. We've worked with 3 diferrents models, combined with 2 differents text vectorizers: Logistic Regression w/ BoW vectorizer (more basic); Linear SVC and Random Forest w/ TFIDF vectorizer. 

For the two last models, we've done cross validation to get the best value for the hyperparameter we've choose to optimize, and tested for balanced and unbalanced class weights. We've choose the accuracy score to evaluated and comapare the models perfomance.


**Results**

- **The best model was the Linear SVC without balance, with 83,2% of accuracy**.
- **All of the models had close results**. The least model, Logistic Regression w/ balance, perfomed with 79% of accuracy: the biggest difference is only 4% of accuracy.
- **The unbalanced models had a better perfomance than the balanced ones**. We've think one of the reasons for that is the fact that we have unbalanced class: when we balanced weights, the accuracy of the minor classes went better, but the major class dicreased. A little variation of the performance in the major class can cause more errors than a larger variation on the minor classes can improve those.

**Considerations**

- For futher work, we must **check other metrics to evaluate the models, like f1-score**, that considers the errors (type I and II) inside the classes. We've tried to use this metric here, but we've had some problems that we couldn't fix. We think that we must change the approach of the label powerset to a binary representation of the classes (one-hot encoding) so that we can use the f1-score.