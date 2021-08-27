xFuzz
============

.. image:: logo.png
    :width: 350px
    :alt: Solidity logo
    :align: center

Cross-contract vulnerabilities are vulnerabilities where more than two contracts are involved by external calls. Cross-contract call itself is generally not vulnerable. However, it is prone to being leveraged by existing vulnerabilities and deceive current vulnerability detection technologies. For example, a cross-contract vulnerability is missed by existing detection tools is shown in the following code.

.. code-block:: Solidity
    :linenos:
    :emphasize-lines: 6,19

    contract Trader {
        TokenSale tokenSale = new TokenSale(); // internal contract

        function combination() {
            tokenSale.buyTokensWithWei();
            tokenSale.buyTokens(beneficiary);
        }
    }

    contract TokenSale {
        TokenOnSale tokenOnSale; // external contract

        function set(address _add) {
            tokenOnSale = TokenOnSale(_add);
        }

        function buyTokens(address beneficiary) {
            if (starAllocationToTokenSale > 0) {
                tokenOnSale.mint(beneficiary, tokens);
            }
        }

        function buyTokensWithWei() onlyTrader {
            wallet.transfer(weiAmount);
        }
    }

The code at **line 6** directs to an external contract *TokenSale* and invokes an uncontroled external call at **line 19**, which can be leveraged to form a cross-contract vulnerability.

These vulnerabilities are firstly reported by `Clairvoyance <https://ieeexplore.ieee.org/document/9270398>`_ . However, this tool only supports detecting cross-contract reentrancy vulnerabilities. Considering other overlooked cross-conract threats by compounding cross-contract calls with existing vulnerabilities, we extend their work by supporting three most dangerous cross-contract vulnerabilities.

We present **xFuzz**, a machine learning guided cross-contract fuzzing framework.

We train a machine leaning model to provide suggestions on deciding a function is whether vulnerable or not. To further effectively guide fuzzers, we propose to combine model predictions and static analysis to guide fuzzers in three ways:

- model predictions help locate vulnerable functions
- identify exploitable paths which are related to internal callers
- identify exploitable paths which are related to external callers 

In this website, we will keep updating suppliment materials. 


Suppliment Materials:
------------------------

.. toctree::
    :maxdepth: 1

    dataset.rst
    ML-model.rst
    guided-fuzzing.rst
    exploits.rst
