---
title: 八大经典排序算法汇总
date: 2017-07-14 11:31:37
tags: Algorithm
---

涵盖了八大排序算法，欢迎优化补充。

```
class Algorithm
{
    //快速排序
    public static function quickSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }
        $left = $right = [];

        for ($i = 1; $i < $len; $i++) {
            if ($arr[$i] < $arr[0]) {
                $left[] = $arr[$i];
            } else {
                $right[] = $arr[$i];
            }
        }
        $left  = self::quickSort($left);
        $right = self::quickSort($right);

        return array_merge($left, [$arr[0]], $right);
    }

    static function array_swap($arr, $l, $r) {
        $arr[$l] = $arr[$l] ^ $arr[$r];
        $arr[$r] = $arr[$l] ^ $arr[$r];
        $arr[$l] = $arr[$l] ^ $arr[$r];

        return $arr;
    }

    public static function bubbleSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        for ($i = 1; $i < $len; $i++) {
            $max_j = $len - $i;
            for ($j = 0; $j < $max_j; $j++) {
                if ($arr[$j] > $arr[$j + 1]) {
                    $arr = self::array_swap($arr, $j, $j + 1);
                }
            }
        }

        return $arr;
    }

    public static function selectionSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        for ($i = 0; $i < $len; $i++) {
            $min_i = $arr[$i];
            $index = $i;
            $min_j = $i + 1;
            for ($j = $min_j; $j < $len; $j++) {
                if ($arr[$j] < $min_i) {
                    $min_i = $arr[$j];
                    $index = $j;
                }
            }
            if ($i < $index) {
                $arr = self::array_swap($arr, $i, $index);
            }
        }

        return $arr;
    }

    public static function insertionSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        for ($i = 0; $i < $len; $i++) {
            $pre_i   = $i - 1;
            $current = $arr[$i];
            while ($pre_i >= 0 && $arr[$pre_i] > $current) {
                $arr[$pre_i + 1] = $arr[$pre_i];
                $pre_i -= 1;
            }
            $arr[$pre_i + 1] = $current;
        }

        return $arr;
    }

    public static function shellSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        $gap   = 1;
        $pre_i = $len / 3;
        while ($gap < $pre_i) {
            $gap = $gap * 3 + 1;
        }

        while ($gap > 0) {
            for ($i = 0; $i < $len; $i++) {
                $temp = $arr[$i];
                $j    = $i - $gap;

                while ($j >= 0 and $arr[$j] > $temp) {

                    $arr[$j + $gap] = $arr[$j];
                    $j -= $gap;
                }

                $arr[$j + $gap] = $temp;
            }

            $gap = floor($gap / 3);
        }

        return $arr;
    }

    public static function mergeSort(&$arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        $mid   = $len >> 1;
        $left  = array_slice($arr, 0, $mid);
        $right = array_slice($arr, $mid);
        static::mergeSort($left);
        static::mergeSort($right);
        if (end($left) <= $right[0]) {
            $arr = array_merge($left, $right);
        } else {
            for ($i = 0, $j = 0, $k = 0; $k <= $len - 1; $k++) {
                if ($i >= $mid && $j < $len - $mid) {
                    $arr[$k] = $right[$j++];
                } elseif ($j >= $len - $mid && $i < $mid) {
                    $arr[$k] = $left[$i++];
                } elseif ($left[$i] <= $right[$j]) {
                    $arr[$k] = $left[$i++];
                } else {
                    $arr[$k] = $right[$j++];
                }
            }
        }

        return $arr;
    }

    public static function countingSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        $count = $sorted = [];
        $min   = min($arr);
        $max   = max($arr);
        for ($i = 0; $i < $len; $i++) {
            $count[$arr[$i]] = isset($count[$arr[$i]]) ? $count[$arr[$i]] + 1 : 1;
        }
        for ($j = $min; $j <= $max; $j++) {
            while (isset($count[$j]) && $count[$j] > 0) {
                $sorted[] = $j;
                $count[$j]--;
            }
        }

        return $sorted;
    }

    public static function heapSort($arr) {
        if (! is_array($arr)) {
            return false;
        }
        global $len;
        $len = count($arr);
        if ($len <= 1) {
            return $arr;
        }

        for ($i = $len; $i >= 0; $i--) {
            self::heapify($arr, $i);
        }

        return $arr;
    }

    static function heapify($arr, $i) {
        $left    = 2 * $i + 1;
        $right   = 2 * $i + 2;
        $largest = $i;

        global $len;
        if ($left < $len && $arr[$left] > $arr[$largest]) {
            $largest = $left;
        }
        if ($right < $len && $arr[$right] > $arr[$largest]) {
            $largest = $right;
        }
        if ($largest != $i) {
            self::array_swap($arr, $i, $largest);
            self::heapify($arr, $largest);
        }
    }
}
```
下面是调用方法
```
$total = 20;
$arr   = [];
for ($i = 0; $i < $total; $i++) {
    $arr[] = rand(-10000, 10000);
}

$quickSort     = Algorithm::quickSort($arr);
$bubbleSort    = Algorithm::bubbleSort($arr);
$selectionSort = Algorithm::selectionSort($arr);
$insertionSort = Algorithm::insertionSort($arr);
$shellSort     = Algorithm::shellSort($arr);
$mergeSort     = Algorithm::mergeSort($arr);
$countingSort  = Algorithm::countingSort($arr);
$heapSort      = Algorithm::heapSort($arr);

print_r([
            '原数组'  => $arr,
            '快速排序' => $quickSort,
            '冒泡排序' => $bubbleSort,
            '选择排序' => $selectionSort,
            '插值排序' => $insertionSort,
            '希尔排序' => $shellSort,
            '归并排序' => $mergeSort,
            '计算排序' => $countingSort,
            '堆排序'  => $heapSort,
        ]);
```
