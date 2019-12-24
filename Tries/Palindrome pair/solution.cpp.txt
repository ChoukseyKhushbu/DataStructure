#include <string>
#include <vector> 
class TrieNode {
	public :
	char data;
	TrieNode **children;
	bool isTerminal;
	int childCount;

	TrieNode(char data) {
		this -> data = data;
		children = new TrieNode*[26];
		for(int i = 0; i < 26; i++) {
			children[i] = NULL;
		}
		isTerminal = false;
		childCount = 0;
	}
};

class Trie {
	TrieNode *root;

	public :
	int count;

	Trie() {
		this->count = 0;
		root = new TrieNode('\0');
	}

	bool insertWord(TrieNode *root, string word) {
		// Base case
		if(word.size() == 0) {
			if (!root->isTerminal) {
				root -> isTerminal = true;
				return true;	
			} else {
				return false;
			}
		}

		// Small Calculation
		int index = word[0] - 'a';
		TrieNode *child;
		if(root -> children[index] != NULL) {
			child = root -> children[index];
		}
		else {
			child = new TrieNode(word[0]);
			root -> children[index] = child;
			root->childCount++;
		}

		// Recursive call
		return insertWord(child, word.substr(1));
	}

	// For user
    void insertWord(string word) {
        if (insertWord(root, word)) {
            this->count++;
        }
    }

    int ischild(TrieNode* root)
    {
        int count=0;
        for(int i=0;i<26;i++)
        {
            if(root->children[i]!=NULL)
                return 1;
        }
        return 0;
    }
    
    bool isPalindrome(string word)
    {
        for(int i=0;i<word.length()/2;i++)
        {
            if(word[i]!=word[word.length()-i-1])
            {
                return false;
            }
        }
        return true;
    }

    bool findnew(TrieNode* root,string word)
    {
        if(ischild(root)==0)
        {
            if(word.size()==0)
            {
                return true;
            }
            else
            {
                return isPalindrome(word);
                // return remainingword(root,"");
            }
        }

        int childindex=word[0]-'a';
        TrieNode* child;
        if(root->children[childindex] != NULL)
        {
            child=root->children[childindex];
            return findnew(child,word.substr(1));
        }
        else
            return false;

    }

    bool findIfPalindromePair(vector<string> arr) {
        // Complete this function
        // Return true or false.

        for(int i=0;i<arr.size();i++)
        {
            string str=arr[i];
            string rev="";
            for(int j=str.length()-1;j>=0;j--)
            {
                rev+=str[j];
            }
            insertWord(rev);
        }
        for(int i=0;i<arr.size();i++)
        {
            if(findnew(root,arr[i]))
            {
                return true;
            }
        }

        return false;

    }
};
