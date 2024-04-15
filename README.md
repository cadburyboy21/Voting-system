# Voting-system
Voting System in solidity


// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract VotingSystem {
    mapping(address => bool) public hasVoted;
    mapping(string => uint256) public votesReceived;
    string[] public candidates;

    event VoteCast(address indexed voter, string candidate);

    constructor(string[] memory _candidates) {
        candidates = _candidates;
    }

    function vote(string memory _candidate) public {
        require(!hasVoted[msg.sender], "You have already voted.");
        bool isValidCandidate = false;
        for (uint256 i = 0; i < candidates.length; i++) {
            if (keccak256(abi.encodePacked(candidates[i])) == keccak256(abi.encodePacked(_candidate))) {
                isValidCandidate = true;
                break;
            }
        }
        require(isValidCandidate, "Invalid candidate.");

        votesReceived[_candidate]++;
        hasVoted[msg.sender] = true;
        emit VoteCast(msg.sender, _candidate);
    }

    function getCandidateVotes(string memory _candidate) public view returns (uint256) {
        return votesReceived[_candidate];
    }
}
