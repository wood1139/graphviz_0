# graphviz_0
用于把几个.dot流程图转成PDF

![Alt text](https://g.gravizo.com/source/custom_mark_g1?https://github.com/wood1139/graphviz_0/blob/master/README.md)
<details> 
<summary></summary>
custom_mark_g1
  digraph g1 {	
		{
			node [shape=point]
			{ rank=same; d1 d2 d3 d4 d5 d6 d7 d8 d9 d10 d11 d12 d13 d14 d15 d16 d17 d18 d19 d20 d21 d22};
			d1->d2->d3->d4->d5->d6->d7->d8->d9->d10->d11->d12->d13->d14->d15->d16->d17->d18->d19->d20->d21->d22 [arrowsize=0];
			d1->d20 [arrowsize=0, color=blue];
			d2->d21 [arrowsize=0, color=red];
			d3->d22 [arrowsize=0, color=green];
		}
		
		{
			node [shape=plaintext]
			{ rank=same; "判断输出1" "判断输出2" "判断输出3" };
			d20->"判断输出1" [color=blue];
			d21->"判断输出2" [color=red];
			d22->"判断输出3" [color=green];
		}

		comment [label="每一次测距后，都结合最近的20次测距结果，进行一次判断输出" shape=plaintext]
		comment->d12 [arrowsize=0 penwidth=0]
	}
custom_mark_g1
</details>


![Alt text](https://g.gravizo.com/source/custom_mark_g2?https://github.com/wood1139/graphviz_0/blob/master/README.md)
<details> 
<summary></summary>
custom_mark_g2
  digraph g2 {
		node [shape=plaintext]
		a [label="用于判断稳态的amp极差阈值"]
		b [label="="]
		c [label="40, amp<=50"]
		d [label="-6.6482e-5 * amp * amp + 0.13296f * amp + 33.518f,  50<amp<1000"]
		e [label="100, amp均值>1000"]
		{{ rank=same; a b d };}

		c->d->e [arrowsize=0 penwidth=0]
	}
custom_mark_g2
</details>


![Alt text](https://g.gravizo.com/source/custom_mark_g3?https://github.com/wood1139/graphviz_0/blob/master/README.md)
<details> 
<summary></summary>
custom_mark_g3
  digraph g3 {
		
		new_frame             [label="新的一次测距结果"]
		judge_stable          [label="稳态判定", shape=diamond]
		judge_dist_range      [label="dist极差>360cm", shape=diamond]
		judge_empty_rain      [label="amp<800\n 且 \n2个以上dist==0 或 5个以上dist<10cm", shape=diamond]
		judge_dist_park       [label="10cm<dist<360cm", shape=diamond]
		judge_dist_far        [label="dist>360cm", shape=diamond]
		judge_amp_800         [label="amp<800", shape=diamond]
		unstable_cnt_reset    [label="非稳态计数清零"]
		unstable_cnt_acc      [label="非稳态计数累加"]
		judge_unstable_cnt    [label="非稳态计数>1200", shape=diamond]
		judge_unstable_cnt_0  [label="非稳态计数>0", shape=diamond]

		status_park      [label="有车", style=filled]
		status_empty     [label="空场", style=filled]
		status_dirty     [label="脏污", style=filled]
		status_rain      [label="雨雪", style=filled]

		status_jump      [label="状态切换", style=filled, color=".7 .3 1.0"]

		new_frame            ->   judge_stable;
		judge_stable         ->   unstable_cnt_acc          [label="N"];
		unstable_cnt_acc     ->   judge_unstable_cnt
		judge_unstable_cnt   ->   status_rain           [label="Y"];

		judge_stable         ->   judge_unstable_cnt_0  [label="Y"];
		judge_unstable_cnt_0 ->   status_jump;

		judge_stable         ->   judge_dist_range      [label="Y"];
		judge_dist_range     ->   status_empty          [label="Y"];
		judge_dist_range     ->   judge_empty_rain      [label="N"];
		judge_empty_rain     ->   status_empty          [label="Y, 雨天特征"];
		judge_empty_rain     ->   judge_dist_park       [label="N"];

		judge_dist_park     ->   status_park           [label="Y"];
		judge_dist_park     ->   judge_dist_far        [label="N"];
		judge_dist_far      ->   status_empty          [label="Y"];
		judge_dist_far      ->   judge_amp_800         [label="N(dist<10)"];
		judge_amp_800       ->   status_empty          [label="Y"];
		judge_amp_800       ->   status_dirty          [label="N"];

		status_dirty        ->   status_park;

		status_jump         ->   unstable_cnt_reset;
		status_park         ->   judge_unstable_cnt_0;
		status_empty        ->   judge_unstable_cnt_0;
	}
custom_mark_g3
</details>


![Alt text](https://g.gravizo.com/source/custom_mark_g4?https://github.com/wood1139/graphviz_0/blob/master/README.md)
<details> 
<summary></summary>
custom_mark_g4
	digraph g4 {
		status_park      [label="有车", style=filled]
		status_empty     [label="空场", style=filled]

		status_park  -> status_empty [label="离场"]
		status_empty -> status_park  [label="进场"]
		status_park  -> status_park  [label="进场（距离变化大于30cm）"]

	}
custom_mark_g4
</details>