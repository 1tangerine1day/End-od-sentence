# End of Sentence
* Using word index
* Using Parts-of-speech

## Example:

Over the growing season when we account for seasonally variable light inhibition and basal respiration rates, our modeling approaches found a cumulative 12.

-> "." is not end of sentance

7% decrease in total ecosystem respiration compared with estimates that do not account for light inhibition in leaves.

-> "." is end of sentance

## model

    class LSTMTagger(nn.Module):

        def __init__(self, embedding_dim, hidden_dim, vocab_size, tagset_size):
            super(LSTMTagger, self).__init__()
            self.hidden_dim = hidden_dim
            self.word_embeddings = nn.Embedding(vocab_size, embedding_dim)
            self.lstm = nn.LSTM(embedding_dim, hidden_dim)
            self.hidden2tag = nn.Linear(hidden_dim, tagset_size)
            self.hidden = self.init_hidden()

        def init_hidden(self):
            return (torch.zeros(1, 1, self.hidden_dim),
                    torch.zeros(1, 1, self.hidden_dim))

        def forward(self, sentence):
            embeds = self.word_embeddings(sentence)
            lstm_out, _ = self.lstm( embeds.view(len(sentence), 1, -1), self.hidden)
            tag_space = self.hidden2tag(lstm_out.view(len(sentence), -1))
            tag_scores = F.log_softmax(tag_space,dim=1)

## Data
crawler from : https://pubmed.ncbi.nlm.nih.gov/
